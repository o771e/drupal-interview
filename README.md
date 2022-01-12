# Drupal Interview Questions &amp; Answers (2022 Update)



### How to load an entity
````
      /** @var \Drupal\flag\Entity\Flag $flag */
      $flag = $this->entityTypeManager->getStorage('flag')->load('join');
````

X-debug
````
$activity = {Drupal\node\Entity\Node} [30]
 fieldsToSkipFromTranslationChangesCheck = {array} [0]
 in_preview = null
 values = {array} [36]
 fields = {array} [20]
 fieldDefinitions = {array} [43]
 languages = {array} [4]
 langcodeKey = "langcode"
 defaultLangcodeKey = "default_langcode"
 activeLangcode = "x-default"
 defaultLangcode = "he"
...
````

### Basic menu types in Drupal 8
* Administration
* Footer
* Main navigation
* Tools
* User account menu

### How to fix issues with permissions in `web/sites/default/files`
````
sudo find files/ -type d -exec chmod 777 {} \;
sudo find files/ -type f -exec chmod 777 {} \;
````

### Discover Existing Hooks

### Drush commands
In order of popularity.  
`drush cr`  
`drush cex -y`  
`drush cim`  
`drush en {module_name}` - enable module  
`drush pmu  {module_name}` - uninstall module  
`drush pm:list`  
`drush core:status`  
`drush core:requirements`  
`drush core:cron`  
`drush en`  
`drush ws` - show php errors in logs  
`drush uli` - one time login  
`drush drush uli --uid=1 --uri=http://jm.localhost`  
`drush sqlc < db.sql`  
`drush sql-dump > dump.sql`  
`drush sql-dump | gzip > drupal_up28052001.sql.gz` - archive sql output  
`drush sql-drop`  
`drush updatedb:status`  
`drush updatedb`  
`drush gunzip < sql_2001_301_4.sql.gz | drush sqlc`  
`drush cex --destination=config`  
`drush eval "print_r(array_keys(\Drupal::entityTypeManager()->getDefinitions()));"`  
`drush pm:security`  
`drush pm:security-php`  
`drush user:login`  

