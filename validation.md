# Validation

## Contents

- [The Basics](#the-basics)
- [Validation Rules](#validation-rules)
- [Retrieving Error Message](#retrieving-error-messages)
- [Error Messages And Views](#error-messages-and-views)
- [Custom Validation Rules](#custom-validation-rules)

<a name="the-basics"></a>
## The Basics

Almost every interactive web application needs to validate data. For instance, a registration form probably requires the password to be confirmed. Maybe the e-mail address must be unique. Validating data can be a cumbersome process. Thankfully, it isn't in Laravel. The Validator class provides as awesome array of validation helpers to make validating your data a breeze. Let's walk through an example:

**Get an array of data you want to validate:**

	$input = Input::all();

**Define the validation rules for your data:**

	$rules = array(
		'name'  => 'required|max:50',
		'email' => 'required|email|unique:users',
	);

**Create a Validator instance and validate the data:**

	$validation = Validator::make($input, $rules);

	if ($validation->fails())
	{
		return $validation->errors;
	}

With the *errors* property, you can access a simple message collector class that makes working with your error messages a piece of cake. Of course, default error messages have been setup for all validation rules. The default messages live at **language/en/validation.php**.

Now you are familiar with the basic usage of the Validator class. You're ready to dig in and learn about the rules you can use to validate your data!

<a name="validation-rules"></a>
## Validation Rules

- [Required](#rule-required)
- [Alpha, Alpha Numeric, & Alpha Dash](#rule-alpha)
- [Size](#rule-size)
- [Numeric](#rule-numeric)
- [Inclusion & Exclusion](#rule-in)
- [Confirmation](#rule-confirmation)
- [Acceptance](#rule-acceptance)
- [Uniqueness & Existence](#rule-unique)
- [E-Mail Addresses](#rule-email)
- [URLs](#rule-url)
- [Uploads](#rule-uploads)

<a name="rule-required"></a>
### Required

**Validate that an attribute is present and is not an empty string:**

	'name' => 'required'

<a name="rule-alpha"></a>
### Alpha, Alpha Numeric, & Alpha Dash

**Validate that an attribute consists solely of letters:**

	'name' => 'alpha'

**Validate that an attribute consists of letters and numbers:**

	'username' => 'alpha_num'

**Validate that an attribute only contains letters, numbers, dashes, or underscores:**

	'username' => 'alpha_dash'

<a name="rule-size"></a>
### Size

**Validate that an attribute is a given length, or, if an attribute is numeric, is a given value:**

	'name' => 'size:10'

**Validate that an attribute size is within a given range:**

	'payment' => 'between:10,50'

> **Note:** All minimum and maximum checks are inclusive.

**Validate that an attribute is at least a given size:**

	'payment' => 'min:10'

**Validate that an attribute is less than a given size:**

	'payment' => 'max:50'

<a name="rule-numeric"></a>
### Numeric

**Validate that an attribute is numeric:**

	'payment' => 'numeric'

**Validate that an attribute is an integer:**

	'payment' => 'integer'

<a name="rule-in"></a>
### Inclusion And Exclusion

**Validate that an attribute is contained in a list of values:**

	'size' => 'in:small,medium,large'

**Validate that an attribute is not contained in a list of values:**

	'language' => 'not_in:cobol,assembler'

<a name="rule-confirmation"></a>
### Confirmation

The *confirmed* rule validates that, for a given attribute, a matching *attribute_confirmation* attribute exists.

**Validate that an attribute is confirmed:**

	'password' => 'confirmed'

Given this example, the Validator will make sure that the *password* attribute matches the *password_confirmation* attribute in the array being validated.

<a name="rule-acceptance"></a>
### Acceptance

The *accepted* rule validates that an attribute is equal to *yes* or *1*. This rule is helpful for validating checkbox form fields such as "terms of service".

**Validate that an attribute is accepted:**

	'terms' => 'accepted'

<a name="rule-unique"></a>
### Uniqueness And Existence

**Validate that an attribute is unique on a given database table:**

	'email' => 'unique:users'

In the example above, the *email* attribute will be checked for uniqueness on the *users* table. Need to verify uniqueness on a column name other than the attribute name? No problem:

**Specify a custom column name for the unique rule:**

	'email' => 'unique:users,email_address'

Many times, when updating a record, you want to use the unique rule, but exclude the row being updated. For example, when updating a user's profile, you may allow them to change their e-mail address. But, when the *unique* rule runs, you want it to skip the given user since they may not have changed their address, thus causing the *unique* rule to fail. It's easy:

**Forcing the unique rule to ignore a given ID:**

	'email' => 'unique:users,email_address,10'

**Validate that an attribute exists on a given database table:**

	'state' => 'exists:states'

**Specify a custom column name for the exists rule:**

	'state' => 'exists:states,abbreviation'

<a name="rule-email"></a>
### E-Mail Addresses

**Validate that an attribute is an e-mail address:**

	'address' => 'email'

> **Note:** This rule uses the PHP built-in *filter_var* method.

<a name="rule-url"></a>
### URLs

**Validate that an attribute is a URL:**

	'link' => 'url'

**Validate that an attribute is an active URL:**

	'link' => 'active_url'

> **Note:** The *active_url* rule uses *checkdnsr* to verify the URL is active.

<a name="rule-uploads"></a>
### Uploads

The *mimes* rule validates that an uploaded file has a given MIME type. This rule uses the PHP Fileinfo extension to read the contents of the file and determine the actual MIME type. Any extension defined in the *config/mimes.php* file may be passed to this rule as a parameter:

**Validate that a file is one of the given types:**

	'picture' => 'mimes:jpg,gif'

> **Note:** When validating files, be sure to use Input::file() or Input::all() to gather the input.

**Validate that a file is an image:**

	'picture' => 'image'

**Validate that a file is less than a given size in kilobytes:**

	'picture' => 'image|max:100'