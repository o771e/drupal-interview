# Drupal Interview Questions &amp; Answers (2022 Update)

### How to debug Drupal with X-Debug
1. Set breakpoints (conditional) in custom and contrib modules, in core.
2. See call stack, step back in calls list.
3. Read comments.
4. Look for examples in current codebase.


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
`drush eval "print_r(array_keys(\Drupal::entityTypeManager()->getDefinitions()));"` - list all available entities 
`drush eval "print_r(\Drupal::getContainer()->getServiceIds());"` - get available services 
`drush pm:security`  
`drush pm:security-php`  
`drush user:login`  
`drush generate` - www.drush.org/latest/commands/generate; https://www.drush.org/latest/generators/all/
`drush route` - see all routes
`drush route --path=/user/1`
`drush route --name=update.status`
`drush scr my_file.php` - run php script in Drupal context
`drush queue:list` - Returns a list of all defined queues.
`drush watchdog:tail` - Tail watchdog messages.


### The list of commonly used static methods
````
\Drupal::currentUser();
\Drupal::routeMatch();
\Drupal::routeMatch()->getRouteName();

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
````PHP
class DownloadFilesBlock extends BlockBase implements ContainerFactoryPluginInterface
````
2. Update __construct() method to receive and store the injected services:
````PHP
public function __construct(array $configuration, $plugin_id, $plugin_definition, FormBuilderInterface $form_builder) {
    parent::__construct($configuration, $plugin_id, $plugin_definition);
    $this->formBuilder = $form_builder;
  }
````
3. Implement create() method and use the service container to pass the services needed by the constructor:
````PHP
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
* HttpKernel - a system which gets the request and asks other systems to produce the requested output (a response object) and sends the response back to the client. The output is generated by a piece of code called the controller. In theory the controller can be either a pure PHP 4 function, a method on an object or even an anonymous function. [More info.](https://symfony.com/doc/current/components/http_kernel.html)
* PHPUnit Bridge
* Polyfill Iconv
* Process
* Routing
* Serializer
* Translation
* Validator
* Yaml

### How to create a custom page programmatically?
1. Create routing file `example/example.routing.yml` and add the below line
```YAML
example.content:
  path: '/example' 
  defaults: 
    _controller: '\Drupal\example\Controller\ExampleController::content' 
    _title: 'Hello World'
  requirements: 
    _permission: 'access content' 
```
2. For the above route the class needs to be placed in `example/src/Controller` directory and with name ExampleController.php. So the full path will look like, `example/src/Controller/ExampleController.php`.
````PHP
namespace Drupal\example\Controller;

use Drupal\Core\Controller\ControllerBase;

/**
 * An example controller.
 */
class ExampleController extends ControllerBase {

  /**
   * Returns a render-able array for a test page.
   */
  public function content() {
    $build = [
      '#markup' => $this->t('Hello World!'),
    ];
    return $build;
  }

}
````

### How to a Twig template to the page controller?
1. Create a Twig Template `module-name-theme-hook.html.twig` in "templates" folder.
````HTML
<!-- Let's output the three variables passed to this template -->

<h1>My lovely twig template.</h1>
<p>You passed this text: <b>{{ variable1 }}</b></p>
<p>You passed this number value: {{ variable2 }}</p>
<p>And you passed this array:</p>

<ul>
  {% for item in variable3 %}
    <li>
       {{ item }} 
    </li>        
  {% endfor %}    
</ul>

<p>Ciao!</p>
````
2. Add a theme hook in `your_module.module`
````PHP
/**
 * Implements hook_theme().
 * 
 * Register a module or theme's theme implementations.
 * The implementations declared by this hook specify how a particular render array is to be rendered as HTML.
 * 
 * See: https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21theme.api.php/function/hook_theme/8.2.x
 * 
 * If you change this method, clear theme registry and routing table 'drush cc theme-registry' and 'drush cc router'.
 */
function module_name_theme($existing, $type, $theme, $path) {

  return [		
    // Name of the theme hook. This is used in the controller to trigger the hook.
    'module_name_theme_hook' => [
      'render element' => 'children',
      // If no template name is defined here, it defaults to the name of the theme hook, ie. module-name-theme-hook.html.twig
      'template' => 'module-name-theme-hook',
      // Optionally define path to Twig template files. Defaults to the module's ./templates/ directory.
      'path' => $path . '/templates',
      // Optionally define variables that will be passed to the Twig template and set default values for them.
      'variables' => [
        'variable1' => 'Yet another default text.',
        'variable2' => 0,
        'variable3' => [0, 0, 0],
      ],
    ],
  ];

}
````
3. Invoke the theme hook from the page controller.
````PHP
namespace Drupal\example\Controller;

use Drupal\Core\Controller\ControllerBase;

/**
 * An example controller.
 */
class ExampleController extends ControllerBase {

  /**
   * Returns a render-able array for a test page.
   */
  public function content() {

    // Do something with your variables here.
    $myText = 'This is not just a default text!';
    $myNumber = 1;
    $myArray = [1, 2, 3];

    return [
      // Your theme hook name.
      '#theme' => 'module_name_theme_hook',      
      // Your variables.
      '#variable1' => $myText,
      '#variable2' => $myNumber,
      '#variable3' => $myArray,
    ];
  }
}
````


### What is namespace?
**namespace** is an identifier of a class in the file.

### What is the use of "use" keyword and usage?
**use** is a keyword to include the class or file. For code performence, use the keyword 'use' in the top of the line to call the class instead of calling the class directly in the function.

### Nginx configuration for running Drupal
https://github.com/perusio/drupal-with-nginx
https://www.nginx.com/blog/8-tips-for-drupal-8-performance/

### The most used hooks
* hook_menu()
* hook_uninstall()
* hook_permission()
* hook_install()
* hook_theme()
* hook_schema()
* hook_help()
* hook_form_alter()
* hook_views_api()
* hook_init()

### Where can I find the documentation of the Core modules?
https://www.drupal.org/docs/core-modules-and-themes/core-modules

### What is the besic structure of Drupal
https://www.drupal.org/docs/core-modules-and-themes/basic-structure-of-drupal

### What is delta in Drupal's tables
https://www.ostraining.com/blog/drupal/delta/

### What is Plugin in Drupal
https://evolvingweb.ca/blog/introduction-services-plugins-and-events-drupal

### What's the difference between image and container

### Get a list of all users and into an array
````php
$query = \Drupal::entityQuery('user');

$uids = $query->execute();
````

### How to reach/obtain the configuration of a module?
````php
$max_depth = \Drupal::config('admin_toolbar.settings')->get('menu_depth');
````

### Get a list of users with certain role
````php
$userlist[] = \Drupal\user\Entity\User::load(1)->get('mail')->value;
$ids = \Drupal::entityQuery('user')
  ->condition('status', 1)
  ->condition('roles', 'guest')
  ->execute();
$users = User::loadMultiple($ids);
foreach ($users as $user) {
  $mail = $user->get('mail')->getString();
  $userlist[] = $mail;
}

````

### List all variables in a given Twig
Just a list of available variables:
````{{ dpm() }}````, ````{{ dpm(_context|keys) }}````  
Global variables available in all Twig templates: `_context`, `_parent`.  
Symfony's dumper is used (must be selected in Devel settings), `dump()` doesn't work.  
For X-debug `{{ devel_breakpoint() }}` is used.



