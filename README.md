# CakePHP Uploader Plugin
File and Image Uploader for CakePHP

# Installing

For the GIT Submodules version, please use the "git" branch.

Use the master branch for the composer version.

Add the following to your composer.json file

```
"repositories": [
        {
            "type": "package",
            "package": {
                "name": "MichaelHoughton/Uploader",
                "version": "dev-master",
                "type": "cakephp-plugin",
                "source": {
                    "url": "https://github.com/MichaelHoughton/cakephp-file-uploader.git",
                    "type": "git",
                    "reference": "master"
                }
            }
        }
    ],
    "require": {
        "MichaelHoughton/Uploader": "dev-master",
        "james-heinrich/phpthumb": "dev-master"
    }
```

2) Run

```
php composer.phar update
```

3) In your app/Config/bootstrap.php file, load the plugin:

```php
CakePlugin::load('Uploader');
```

# Configuration

```php
public $actsAs = array('Uploader.ImageUpload' => array(
                            'yourFieldName' => array(
                                'required'               => false,
                                'directory'           => 'img/uploads/',
                                'allowed_mime'        => array('image/jpeg', 'image/pjpeg', 'image/gif', 'image/png'),
                                'allowed_extension'   => array('.jpg', '.jpeg', '.png', '.gif'),
                                'allowed_size'        => 2097152,
                                'random_filename'     => true,
                                'resize' => array(
                                    'thumb' => array(
                                        'directory' => 'img/uploads/thumbs/',
                                        'phpThumb' => array(
                                            'far' => 1,
                                            'bg'  => 'FFFFFF',
                                            'zc' => 0
                                        ),
                                        'width' => 230,
                                        'height' => 150
                                    ),
                                    'max' => array(
                                        'directory' => 'img/uploads/thumbs/',
                                        'phpThumb' => array(
                                            //'far' => 1,
                                            //'bg'  => 'FFFFFF',
                                            'zc' => 0
                                        ),
                                        'width' => 400,
                                        'height' => 300
                                    )
                                )
                            )
                        )
                    );
```

#Explaining the Configuration

yourFieldName = this is the name of the database field you want to save the image in, e.g. image

You will see a variable called "phpThumb" in the array.  You can set different PHPTHUMB options, these can be found here:
http://phpthumb.sourceforge.net/demo/docs/phpthumb.readme.txt

There are a couple of other options you can call when required:

If you want to have a check box to delete the image, you can set that like this:

```php
<?php echo $this->Form->checkbox('yourFieldName.delete', array('label' => 'Delete this image')); ?>
```

This is handy if you want to say give the user the option to delete the image already loaded.

If you want to load an image outside the behaviour, or perhaps you just want to load an image name without uploading an image, just add the following to your $this->request->data array before saving the data:

```php
$this->request->data['Model']['noUpload'] = true;
```

This will ignore the behaviour.

The behavior will also automatically delete the image file (and resized files) when you delete that row.  It will first check that that image file name isn't used in other rows in the model, before deleting the file for good!

Finally - you will need to ensure you have the field in your database table, e.g. in your table, you need to have a field called "yourFieldName", otherwise, the behaviour won't work.