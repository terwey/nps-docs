FacebookNewscoopBundle
======================

Usefull sevices for integration Newscoop and Facebook.

Facebook caches shared urls for better performance. It can cause some problems at the time we want to change title, decscripton of our article because we still have an older version of our article. This is why this plugin is designed for.

**Purpose:** Clear an article cache on Facebook.

Installing Newscoop Facebook Plugin Guide
-------------
Installation is a quick process:


1. Installing plugin through our Newscoop Plugin System
2. That's all!

### Step 1: Installing plugin through our Newscoop Plugin System
Run the command:
``` bash
$ php application/console plugins:install "ahs/facebook-newscoop-bundle" --env=prod
```
Plugin will be installed to your project's `newscoop/plugins/AHS` directory.


### Step 2: That's all!
Go to an article edition in Newscoop to see Newscoop Facebook Plugin on the right sidebar in action.

Also read more about [Lifecycle Subscriber Managing](https://wiki.sourcefabric.org/display/NPS/Lifecycle+Subscriber+Managing).

Plugin in action:
-------
When you enter an article edition in Newscoop, plugin will automatically look into database and check if there are already informations about that article, if not then it will connect to Facebook Graph API and download all necessary informations. Next, informations will be inserted into database.


To simply clear article cache on Facebook, just click `Clear article cache on Facebook` button.
After you click It, proper message will show up (look at **Screen 2**).

If connection to Facebook success, status message on **Screen 1** will show up.

If for some reasons connection to Facebook won't be possible, we will see error message (look at **Screen 3**)

![Screen 1](http://i42.tinypic.com/30k65x0.png)

**Screen 1:** General Plugin view.
![Screen 2](http://i42.tinypic.com/2qve24x.png)
**Screen 2:** Connecting message after button click.
![Screen 3](http://i42.tinypic.com/6tp9nl.png)
**Screen 3:** Connection to Facebook failed - status message.

License
-------

This bundle is under the GNU General Public License v3. See the complete license in the bundle:

    LICENSE.txt - http://www.gnu.org/licenses/gpl-3.0.txt

About
-------
FacebookNewscoopBundle is a [Sourcefabric o.p.s](https://github.com/sourcefabric) initiative.

[1]: http://getcomposer.org/doc/00-intro.md
[packagist]: https://packagist.org/
[github]: https://github.com/
[satis]: https://github.com/composer/satis
