# Templating

## Contents

- [The Basics](#the-basics)
- [Blade Template Engine](#blade-template-engine)
- [Sections](#sections)

<a name="the-basics"></a>
## The Basics

Your application probably uses a common layout across most of its pages. Manually creating this layout within every controller action can be a pain. Specifying a controller layout will make your develompent much more enjoyable. Here's how to get started:

**Specify a "layout" property on your controller:**

	class Base_Controller extends Controller {

		public $layout = 'layouts.common';

	}

**Access the layout from the controllers' action:**

	public function action_profile()
	{
		$this->layout->nest('content', 'user.profile');
	}

> **Note:** When using layouts, actions do not need to return anything.

<a name="blade-template-engine"></a>
## Blade Template Engine

Blade makes writing your views pure bliss. To create a blade view, simply name your view file with a ".blade.php" extension. Blade allows you to use beautiful, unobtrusive syntax for writing PHP control structures and echoing data. Here's an example:

**Echoing a variable using Blade:**

	Hello, {{$name}}.

**Creating loops using Blade:**

	<h1>Comments</h1>

	@foreach ($comments as $comment)
		The comment body is {{$comment->body}}.
	@endforeach

**Other Blade control structures:**

	@if (count($comments) > 0)
		I have comments!
	@else
		I have no comments!
	@endif

	@for ($i =0; $i < count($comments) - 1; $i++)
		The comment body is {{$comments[$i]}}
	@endfor

	@while ($something)
		I am still looping!
	@endwhile

<a name="sections"></a>
## Sections

View sections provide a simple way to inject content into layouts from nested views. For example, perhaps you want to inject a nested view's needed JavaScript into the header of your layout. Let's dig in:

**Creating a section within a view:**

	<?php Section::start('scripts'); ?>
		<script src="jquery.js"></script>
	<?php Section::stop(); ?>

**Rendering the contents of a section:**

	<head>
		<?php echo Section::yield('scripts'); ?>
	</head>

**Using Blade short-cuts to work with sections:**

	@section('scripts')
		<script src="jquery.js"></script>
	@endsection

	<head>
		@yield('scripts')
	</head>