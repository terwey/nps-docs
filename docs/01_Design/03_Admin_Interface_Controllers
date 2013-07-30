## Provide admin controllers

All we need is a simple controller (ex. ```Newscoop\ExamplePluginBundle\Controller\DefaultController```) with action and routing.
In plugins admin controllers we can use twig (also smarty) as template engine. You can see how extend default admin layout (header + menu+footer) here: ```Resources/views/Default/admin.html.twig```

### Provide plugin menu in Newscoop admin menu:

For Newscoop menu we use [KNP/Menu][KNP/Menu] library (with [KNP/MenuBundle][KNP/MenuBundle]), so extending o=newscoop menu is realy easy.

We need only one service (and declaration in services) - declaration:

```
    newscoop_example_plugin.configure_menu_listener:
        class: Newscoop\ExamplePluginBundle\EventListener\ConfigureMenuListener
        tags:
          - { name: kernel.event_listener, event: newscoop_newscoop.menu_configure, method: onMenuConfigure }
```

and menu configuration litener 

```
// EventListener/ConfigureMenuListener.php
<?php
namespace Newscoop\ExamplePluginBundle\EventListener;

use Newscoop\NewscoopBundle\Event\ConfigureMenuEvent;

class ConfigureMenuListener
{
    public function onMenuConfigure(ConfigureMenuEvent $event)
    {
        $menu = $event->getMenu();
        $menu[getGS('Plugins')]->addChild(
            'Example Plugin', 
            array('uri' => $event->getRouter()->generate('newscoop_exampleplugin_default_admin'))
        );
    }
}
```

and it's it - you should have your plugin menu in newscoop menu.