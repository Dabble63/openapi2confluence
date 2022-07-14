# OpenAPI2Confluence

OpenAPI2Confluence is a simple method of converting Swagger openapi yaml or json files to a format that can be inserted into a Confluence WIKI page.

## Example 1:

    node handlebars openapi.json <api.confluence.mustache >/tmp/output

## Example 2:

    ./handlebars  <(yq -o=json openapi.yaml) <api.confluence.mustache >/tmp/output

* The first example converts an openapi definition in json format to /tmp/output. To insert this in a Confluence page you should edit the page and click on <> in the top right corner. Now copy the contents of /tmp/output into the page and press apply.
* The second example is similar to the first but uses yq (https://github.com/mikefarah/yq) to convert the yaml file to json format before running handlebars on it. The syntax assumes that it is being run under bash and that the handlebars file is executable.


To run the converter you will need to have nodejs installed. I have tested it on version 17.3.0. 

After downloading the files you should run *npm install* to install handlebars, yargs and markdownit.

## How it works

The handlebars program (based on https://www.npmjs.com/package/handlebars-cmd) simply runs handlebars on a template file (api.confluence.mustache).

There are four, one line,  routines that were required because vanilla handlebars does not support things like testing the value of a string.
They are:

        hbs.registerHelper('toJSON', function(obj) {
            return JSON.stringify(obj, null, 3);
        });        
        hbs.registerHelper('eq', function(a,b) {
            return a == b;
        });        
        hbs.registerHelper('markdownit', function(x) {
            return md.render(x);
        });        
        hbs.registerHelper('uc', function(x) {
            return x.toUpperCase();
        });        
        
All of the styles are inline or standard Confluence classes because I did not see a way to add a class definition to a Confluence page. 

# Known Limitations

* Only JSON formatting of parameters and examples is properly supported. I had no need to support XML or other formats.
* There is no generation of sample calls
* There is no ability to test run calls.

The last two limitations would probably require Confluence plugins. I have tried to avoid using any plugins. The only plugins that I have used are expand and tabs (container and page).

As there is very little code it should be easy to modify to suite your particular requirements. If you think that what you have built may be useful to someone else then please contribute your changes to this project.
