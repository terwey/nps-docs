## Manage plugin lifecycle

The best way for plugin lifecycle management is registering event subscriber. Events lifecycle consists of 3 events:

    - plugin.install
    - plugin.remove
    - plugin update

This is example of simple event subscriber class:

```
// ExamplePluginBundle/EventListener/LifecycleSubscriber.php
<?php
namespace Newscoop\ExamplePluginBundle\EventListener;

use Symfony\Component\EventDispatcher\EventSubscriberInterface;
use Newscoop\EventDispatcher\Events\GenericEvent;

/**
 * Event lifecycle management
 */
class LifecycleSubscriber implements EventSubscriberInterface
{
    public function install(GenericEvent $event)
    {
        // do something on install
    }

    public function update(GenericEvent $event)
    {
        // do something on update
    }

    public function remove(GenericEvent $event)
    {
        // do something on remove
    }

    public static function getSubscribedEvents()
    {
        return array(
            'plugin.install' => array('install', 1),
            'plugin.update' => array('update', 1),
            'plugin.remove' => array('remove', 1),
        );
    }
}
```

Next step is registering this class in Event Dispatcher:

```
// ExamplePluginBundle/Resources/config/services.yml
services:
    newscoop_example_plugin.lifecyclesubscriber:
        class: Newscoop\ExamplePluginBundle\EventListener\LifecycleSubscriber
        tags:
            - { name: kernel.event_subscriber}

```
Subscriber can have access for all registered in container services (```php application/console container:debug```), only thing what you must to is passing services as argument:

```
// ExamplePluginBundle/Resources/config/services.yml
services:
    newscoop_example_plugin.lifecyclesubscriber:
        class: Newscoop\ExamplePluginBundle\EventListener\LifecycleSubscriber
        arguments:
            - @em
        tags:
            - { name: kernel.event_subscriber}

```
and using it in your service (subscriber):

```
// ExamplePluginBundle/EventListener/LifecycleSubscriber.php
...
class LifecycleSubscriber implements EventSubscriberInterface
{
    private $em;

    public function __construct($em) {
        $this->em = $em;
    }
    ...

```
In subscriber included in this plugin you can find exaple of database updating (based on doctrine entities and schema tool)
