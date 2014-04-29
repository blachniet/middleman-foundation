# Middleman-Foundation #

The following describes how I added [Zurb's Foundation](http://foundation.zurb.com) to the Middleman HTML5 Boilerplate template. The [Middleman Skeleton for Zurb Foundation](https://github.com/axyz/middleman-zurb-foundation) was used as a guide for building this guide, and is great if you don't want to have to do this all yourself.

## Middleman Template and Bower Setup ##

Initialize the project with the HTML5 Boilerplate.

    > middleman init middleman-foundation --template=html5
    > cd middleman-foundation

Create a `.bowerrc` file to specify a the install location for bower components

    // .bowerrc
    {
        "directory" : "source/bower_components"
    }

Initialize bower for the project.

    > bower init

Install [Foundation](http://foundation.zurb.com).

    > bower install foundation

Add the Compass configuration to `config.rb`.

    # config.rb
    compass_config do |config|
      # Require any additional compass plugins here.
      config.add_import_path "bower_components/foundation/scss"

      config.output_style = :compact
    end

Add the bower directory to the Sprockets asset path in the `config.rb`.

    # config.rb
    # Add bower's directory to sprockets asset path
    after_configuration do
      @bower_config = JSON.parse(IO.read("#{root}/.bowerrc"))
      sprockets.append_path File.join "#{root}", @bower_config["directory"]
    end

## Replace CSS with SCSS ##

Delete `source/css/main.css` and `source/css/normalize.css`. In their place add a new file, `all.css.scss`.

    # all.css.scss
    @import "normalize.scss";
    @import "foundation.scss";

Replace the stylesheet references in the `<head>` of `source/layouts/layout.erb` with the `all.css.scss`.

    <!-- source/layouts/layouts.erb -->
    <%= stylesheet_link_tag "all" %>

## Use Modernizr from Bower ##

Create a `source/js/modernizr.js` with the following content to include the modernizr source from the installed bower components.

    // source/js/modernizr.js
    //= require modernizr/modernizr

Then replace the modernizr reference in `layouts.erb`'s head with `<%= javascript_include_tag "modernizr" %>`.

## Use Other Javascripts from Bower ##

Delete all files (except the new `modernizr.js`) in `source/js/`, and create `all.js` with the following contents to include necessary javascripts from the bower components.

    // source/js/all.js
    //= require jquery/dist/jquery
    //= require foundation/js/foundation.min

Then, update `layouts.erb` to reference your new `all.js` and remove the old javascript references.

    <!-- source/layouts/layout.erb -->
    <%= javascript_include_tag  "all" %>