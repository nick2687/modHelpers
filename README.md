## modHelpers
Library of the helpfull functions for MODX.

Available functions:

* url() - make an url.
* redirect() - redirect to the url or site page if the id is passed.
* forward() - forwards the request to another resource without changing the URL.
* abort() - redirect to the error page.
* config() - manage the config settings.
* session() - manage the session using dot notation.
* cache() - manage the cache.
* parents() - gets all of the parent resource ids for a given resource. 
* children() - gets all of the child resource ids for a given resource.
* pls() - to work with placeholders.
* pls_delete() - removes the specified placeholders.
* lang() - to work with lexicon records.
* table_name() - gets the table name of the specified class.
* columns() - gets select columns from a specific class for building a query.
* email() - send email.
* email_user() - send email to the specified user.
* str_clean() - sanitize the string.
* quote() - quote the string.
* escape() - escapes the provided string using the platform-specific escape character.
* css() - register CSS to be injected inside the HEAD tag of a resource.
* script() - register JavaScript to be injected inside the HEAD tag or before the closing BODY tag. Available the script attributes "async" and "defer".
* html() - register HTML to be injected inside the HEAD tag or before the closing BODY tag.
* chunk() - gets the specified chunk. 
* snippet() - runs the specified snippet.
* processor() - runs the specified processor.
* is_auth() - determines if this user is authenticated in a specific context.
* is_guest() - determines if the user is a guest.
* can() - returns true if user has the specified policy permission.
* resource_id() - gets the id of the current resource. 
* template_id() - gets the template id of the current resource.
* user_id() - gets the id of the current user. 
* tv() - gets the specified TV of the current resource. 
* object() - to work with objects of MODX.
* collection() - to work with collections.
* resource() - works with a resource object.
* resources() - works with a resource collection.
* user() - works with a user object.
* users() - works with a user collection.
* object_exists() - checks if the specified object exists.
* user_exists() - checks if the specified user exists.
* resource_exists() - checks if the specified resource exists.
* is_email() - validates the email.
* is_url() - validates the url.
* log_error() — logs to the error log for the ERROR log level.
* log_warn() — logs to the error log for the WARN log level.
* log_info() — logs to the error log for the INFO log level.
* log_debug() — logs to the error log for the DEBUG log level.
* context() - gets the specified property of the current context. By default, returns the key.
* query() - runs the specified SQL query.
* memory() - returns the formatted string of the amount of memory allocated to PHP.
* img() - prepares the HTML tag "img".
* faker() - creates a faked data.
* [load_model()](./core/components/modhelpers/docs/en/load_model.md) - loads a model for a custom table.


### Examples
**Check the user exists**
```
if (user_exists(['email'=>'admin@mail.com']) {
    // Exists
}
```

**Get the data from the cache**
```
//Gets the data from the file *core/cache/my_data/key.cache.php*. 
$value = cache('key', 'my_data');
// Or 
$value = cache()->get('key', 'my_data');
```

**Send an email**
```
email('pussycat@mail.ru', 'Subject','Email content');
// To the user
email_user('admin', $subject, $content); 
// or use the user id
email_user(5, $subject, $content);
// or like that
email()->to('someemail@.gmail.com')->cc('copymail@mail.com')->subject('Hello')->content('Content')->attach('path/to/file.jpg')->send();
```

**Run a snippet and save the result to the cache**
```
$output = snippet('mySnippet', $params, 60); // store the snippet result for 60 seconds
```

**Run a snippet from file**
```
$output = snippet(MODX_CORE_PATH . 'elements/snippets/mysnippet.php', $params);
```

**Get a chunk from file**
```
$output = chunk(MODX_CORE_PATH . 'elements/chunks/mychunk.html', $placeholders);
```

**Redirect**
```
redirect($url);
//To the resource with the id = 5
redirect(5);
```

**The latest resource**
```
$resourceObject = resource()->last(); // Resource object
$resourceArray = resource()->last()->toArray(); // Resource data as array
```

**The last 10 resources**
```
$resObjects = resources()->last(10); 
```

**Array of the resource pagetitles of the parent with the id = 20.**
```
$titles = resources()->where(['parent'=>20])->get('pagetitle'); // array('pagetitle 1', 'pagetitle 2', 'pagetitle 3')
```
**Use a Closure for child resources of the category with the id = 20.**
```
return resources()->where(['id:IN'=>children(20)])->each(function($resource, $idx) {return "<div>{$idx}. " . $resource['pagetitle'] . "</div>";}); 
```
**Set a value to the session**
```
session('key1.key2', 'value'); // => $_SESSION['key1']['key2'] = $value;
```
**Get the value from session**
```
$value = session('key1.key2');  // $value = $_SESSION['key1']['key2']
```

**Validates the email**
```
if (is_email($_POST['email'])) {
   // Valid
}
```
**Remove child resources of the resource with the id = 10**
```
resources()->where(['parent'=>10])->remove();
```
**Count blocked users**
```
$count = users()->profile()->where(['Profile.blocked'=>1])->count();
```
**Load script with the async attribute**
```
script('/path/to/script.js', 'async'); // <script async type="text/javascript" src="/path/to/script.js"></script>
```
**Get an array of users**
```
// Can use the prepared query
$userArray = query('select * from ' . table_name('modUser'). ' WHERE id < ?')->execute(( (int) $_POST['user_id']);
```
**Log error to the error log**
```
log_error($array); // The array wil be converted to a string using print_r().
log_error($message, 'HTML'); // Show message on the page.
```
**Get the list of the pagetitles**
```
return resources()->where(['id:IN'=>children(5)])->each(function($resource, $idx){ return "<li>{$idx}. ".$resource['pagetitle']."</li>";});
```
**Get users which are members of the "Manager" group**
```
$usersArray = users()->members('Managers')->toArray();
// Get all users from "ContentManagers" and "SaleManagers" groups 
$users = users()->members('%Managers')->get();
foreach($users as $user) {
  echo $user->username;
}
```
**Add the first ten users to the "Manager" group**
```
users()->whereIn('id',range(1,10))->joinGroup('Manager');
```
**Make a list of faked news (using a snippet)**
```
return collection(10)->each(function($item, $idx, $modx){return "<div>" . faker()->date() . img(faker()->imageUrl(500,300),['class'=>'img-news']) . '<br>' . faker()->text(700) . '</div>';});
```
  
[Russian documentation](https://modzone.ru/documentation/modhelpers/).