
# Routing
- A route can return many different types
	- A view (HTML Views - Traditional Web)
	- A string/array/object  (JSON/API Responses - Modern Standard)
	- Redirects
		- A redirect tells the client (browser) to immediately make a new request to a different URL.
	- Raw/File Responses 
		- Used for serving files, downloading content, or sending back raw text.


# Blade templating
- Blade now uses component syntax instead of the injection syntax I was using before.
	- Create a component folder ( /resources/views/components)
	- Create a blade file. Ex `layout.blade.php`
	- Use it in other blade views as `<x-<component-name>> </x-<component-name>>
	  Ex: <x-layout> </x-layout>
	- src: https://laracasts.com/series/30-days-to-learn-laravel-11/episodes/3
- Use  `{{ $slot }}` in the layout file to indicate where do you want the children of the layout component to be placed.
- Blade double curly braces are se same as a regular `echo`
  `{{ $slot }}` ==> `<?php  echo $slot ?>`