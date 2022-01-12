# Drupal Interview Questions &amp; Answers (2022 Update)

### How to find out what a class can do?
Read its PHPDoc block comments, duh!

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


### The list of commonly used static methods
````
\Drupal::currentUser();
\Drupal::routeMatch();

````

### Injecting Dependencies to a Service
1. Add a file `{module_name}.services.yml` with code.  
2. Add arguments to service declaration under the key "arguments" and add the symbol "@" before the service name:  
````
services:
  {name}.event_subscriber:
    class: Drupal\{module_name}\EventSubscriber\SomeNameEventSubscriber
    arguments: ['@logger.factory', '@entity_type.manager']
    tags:
      - {name: event_subscriber}
````
3. Update `__construct()` method to receive and store the injected services, like:
````
public function __construct(LoggerChannelFactoryInterface $loggerFactory, EntityTypeManagerInterface $entity_type_manager) {
  $this->logger = $loggerFactory->get('download_files');
  $this->entityTypeManager = $entity_type_manager;
}
````

### Injecting Dependencies to a Block
1. Implement Drupal\Core\Plugin\ContainerFactoryPluginInterface in the class:
````
class DownloadFilesBlock extends BlockBase implements ContainerFactoryPluginInterface
````
2. Update __construct() method to receive and store the injected services:
````
public function __construct(array $configuration, $plugin_id, $plugin_definition, FormBuilderInterface $form_builder) {
    parent::__construct($configuration, $plugin_id, $plugin_definition);
    $this->formBuilder = $form_builder;
  }
````
3. Implement create() method and use the service container to pass the services needed by the constructor:
````
public static function create(ContainerInterface $container, array $configuration, $plugin_id, $plugin_definition) {
    return new static(
      $configuration,
      $plugin_id,
      $plugin_definition,
      $container->get('form_builder')
    );
  }
````
(Here is the PHP theory behind this upper part - `return new static` - https://www.php.net/manual/en/language.oop5.decon.php#example-228 )


### What are all the symphony components used by Drupal 8?
* ClassLoader
* Console
* CssSelector
* Debug
* DependencyInjection
* EventDispatcher
* HttpFoundation
* HttpKernel
* PHPUnit Bridge
* Polyfill Iconv
* Process
* Routing
* Serializer
* Translation
* Validator
* Yaml