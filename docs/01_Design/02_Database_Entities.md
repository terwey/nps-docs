## Create plugin entities

In plugins and in Newscoop we use Doctrine2 for database entities management, so if you know Doctrine2 then you will know how to work with entities in plugins. If you don't know about Doctrine2 then this is great oportunity - [read more here][doctrine]

Few important things:

* You can get entity manager from newscoop container (in plugin controlle simply use ```$this->container->get('em');```)
* We use full FQN notation ex. ```$em->getRepository('Newscoop\ExamplePluginBundle\Entity\OurEntity');``` 