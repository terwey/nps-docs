Plugin Hooks
======================

As you may already know, there are two types of events in our **Newscoop Plugin System**: [lifecycle events](Lifecycle_Subscriber_Managing) and plugin hooks. Hooks are provided by Newscoop to allow your plugins to 'hook into' the rest of Newscoop platform.

1. Which screens are able to use hooks?
-------------

Hooks are currently implemented in the following PHP admin files (directory: `newscoopRoot/admin-files/`):
````
issues/edit.php
sections/edit.php
articles/edit_html.php
system_pref/index.php
system_pref/do_edit.php
pub/pub_form.php
````
2. Hooks implementation:
-------------
Hooks are implemented in files listed in point **1.** as follows:
````
//ex. 1
//newscoop/admin-files/articles/edit_html.php:
<?php 
    echo \Zend_Registry::get('container')->getService('newscoop.plugins.service')
        ->renderPluginHooks('newscoop_admin.interface.article.edit.sidebar', null, array(
            'article' => $articleObj, 
            'edit_mode' => $f_edit_mode
        ));
?>

//ex. 2
//newscoop/admin-files/pub/pub_form.php:
<?php 
    echo \Zend_Registry::get('container')->getService('newscoop.plugins.service')
        ->renderPluginHooks('newscoop_admin.interface.publication.edit', null, array(
            'publication' => $publicationObj
        ));
?>
````
3. How to create hook in your plugin?
-------------
Let's define our hook as a service (plugin will show up in `articles/edit_html.php` file):
````
//Resources/config/services.yml
newscoop_example_plugin.hooks.listener:
        class:     "Newscoop\ExamplePluginBundle\EventListener\HooksListener"
        arguments: ["@service_container"]
        tags:
          - { name: kernel.event_listener, event: newscoop_admin.interface.article.edit.sidebar, method: sidebar }
````

It's not so complicated as you may think. In `EventListener` folder of your plugin directory: `ex. /Newscoop/ExamplePluginBundle/` you must create `HookListener.php` file (name must match name we specified in `services.yml`):

````php
<?php

namespace Newscoop\ExamplePluginBundle\EventListener;

use Symfony\Component\HttpFoundation\Request;
use Newscoop\EventDispatcher\Events\PluginHooksEvent;

class HooksListener
{
    private $container;

    public function __construct($container)
    {
        $this->container = $container;
    }

    public function sidebar(PluginHooksEvent $event)
    {
        $response = $this->container->get('templating')->renderResponse(
            'NewscoopExamplePluginBundle:Hooks:sidebar.html.twig',
            array(
                'pluginName' => 'ExamplePluginBundle',
                'info' => 'This is response from plugin hook!'
            )
        );

        $event->addHookResponse($response);
    }
}
````
Method `sidebar()` takes an event of `PluginHooksEvent` type as parameter.

[PluginHooksEvent.php](https://github.com/sourcefabric/Newscoop/blob/master/newscoop/library/Newscoop/EventDispatcher/Events/PluginHooksEvent.php) class collect Response objects from plugins admin iterface hooks: 


Another step is to create view file in your `Resources/views/` directory of your plugin.

Let's create `Hooks` folder we set in our HooksListener (`NewscoopExamplePluginBundle:Hooks:sidebar.html.twig`)

Inside this folder let's create view for our action in Listener: `sidebar.html.twig`.
This view file is our plugin view and it will show up in files where Plugin Hooks are implemented. We can place HTML/JS/Twig etc. code there:

````
<div class="articlebox" title="{{ pluginName }}">
    <p>{{ info }}</p>
</div>
````
That's it. Plugin response from our hook will show up in article edition view:

![Screen1](http://i41.tinypic.com/16a1j85.png)
