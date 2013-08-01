Plugin Hooks
======================

As you may already know, there are two types of events in our **Newscoop Plugin System**: [lifecycle events](Lifecycle_Subscriber_Managing) and plugin hooks. Hooks are provided by Newscoop to allow your plugins to 'hook into' the rest of Newscoop platform.

1. Which screens are able to use hooks?
-------------

Hooks are currently implemented in following PHP admin files:
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

It's not so complicated as you may think. In `Controller` folder of your plugin directory: `ex. /Newscoop/ExamplePluginBundle/` you have to create `HookController.php` file (of course you can choose your own name):

````php
<?php

namespace Newscoop\ExamplePluginBundle\Controller;

use Symfony\Component\HttpFoundation\Request;
use Newscoop\EventDispatcher\Events\PluginHooksEvent;

class HooksController
{
    private $container;

    public function __construct($container)
    {
        $this->container = $container;
    }

    public function sidebarAction(PluginHooksEvent $event)
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
Action `sidebarAction()` parameter takes an event of `PluginHooksEvent` type.

`PluginHooksEvent.php` class collect Response objects from plugins admin iterface hooks: 

````php
<?php
/**
 * @author Paweł Mikołajczuk <pawel.mikolajczuk@sourcefabric.org>
 * @package Newscoop
 * @copyright 2013 Sourcefabric o.p.s.
 * @license http://www.gnu.org/licenses/gpl-3.0.txt
 */

namespace Newscoop\EventDispatcher\Events;

use Symfony\Component\EventDispatcher\GenericEvent as SymfonyGenericEvent;
use Symfony\Component\HttpFoundation\Response;

/**
 * PluginHooksEvent class.
 *
 * Collect Response objects from plugins admin iterface hooks.
 */
class PluginHooksEvent extends SymfonyGenericEvent
{   
    /**
     * Array with Response objects from hooks
     * @var array
     */
    public $hooksResponses = array();

    /**
     * Add Response object to event
     * @param Response $response
     */
    public function addHookResponse(Response $response)
    {   
        $this->hooksResponses[] = $response;
    }

    /**
     * Get all stored Response objects from event
     * @return array
     */
    public function getHooksResponses()
    {
        return $this->hooksResponses;
    }
}
````
Another step is to create view file in your `Resources/views/` directory of your plugin.

Let's create `Hooks` folder (if our Controller name is `HookController` then folder name in `Resources/views/` directory should be `Hook`).

Inside this folder let's create view for our action in Controller: `sidebar.html.twig`.
This view file is our plugin view and it will show up in files where Plugin Hooks are implemented (look at point **1.**). We can place Html/JS/Twig etc. code there:

````
<div class="articlebox" title="{{ pluginName }}">
    <p>{{ info }}</p>
</div>
````
