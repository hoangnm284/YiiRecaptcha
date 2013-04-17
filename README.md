YiiRecaptcha
============

Yii plugin for using recaptcha-php-1.11

Useful resources:
 - http://www.google.com/recaptcha
 - https://developers.google.com/recaptcha/intro

How to use this plugin
- Go to http://www.google.com/recaptcha to signup and get public and private key for your website
- Copy folder YiiRecaptcha into extensions folder of Yii framework
- In config/main.php file, add the following code into application components part

```php
		'recaptcha'=>array(
			'class'=>'ext.YiiRecaptcha.YiiRecaptcha',
			'public_key'=>'<your public key>',
			'private_key'=>'<your prive key>',
		),
```

- In you view file, add the following code insite your form widget:

```php
 	<?php 
	echo Yii::app()->recaptcha->recaptcha_get_html();
	echo $form->error($model,'Recaptcha');
	?>
```

- Rewrite validate function on your model like below:

```php
	public function validate(){
		$result = parent::validate();
		
		$resp = Yii::app()->recaptcha->recaptcha_check_answer (null,
	                        $_SERVER["REMOTE_ADDR"],
	                        $_POST["recaptcha_challenge_field"],
	                        $_POST["recaptcha_response_field"]);
		if (!$resp->is_valid){
			$this->addError("Recaptcha", $resp->error);
			return false;
		} else {
			return $result;
		}
	}
```

