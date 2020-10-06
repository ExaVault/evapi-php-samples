# evapi-php-samples

## Introduction
Welcome to the sample code for ExaVault's PHP code library, which demonstrates how to use various aspects of our API with your ExaVault account. The PHP code library is available as a composer package and [on Github](https://github.com/ExaVault/evapi-php). The library is generated from our API's [public swagger YAML file](https://www.exavault.com/api/docs/evapi_2.0_public.yaml)

## Requirements

To run these scripts, you'll need PHP 5.5 (or greater) installed as well as [composer](https://getcomposer.org). 

You will also need an ExaVault account as well as and an API key and access token.

Some of the sample scripts will assume your account contains the **ExaVault Quick Start.pdf** file and the **Sample Files and Folders** folder, which come pre-loaded with a new account. You may need to make changes to `sample_upload_files.php` and `sample_download_csv_files.php` if those files are not present.

## Running Your First Sample
**Step 1 - Install dependencies** 

You should use composer to add the ExaVault PHP library to your project by running this command in your project folder:

```bash
% composer require exavault/evapi-php 
```

This will install all the dependencies you need. It may take a while, but eventually you will see something like:

```
Using version ^2.0 for exavault/evapi-php
./composer.json has been created
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: ...
  .... other information for various dependencies ...
Writing lock file
Generating autoload files
```

When composer says `Generating autoload files` all of the dependencies have been successfully installed and you should be able to move onto the next step.

**Step 2 - Get your API Credentials** 

The next step is to generate an API key and token from your ExaVault account. You'll need to log into the ExaVault web file manager, as an admin-level user, to get the API key and access token. See [our API reference documentation](https://www.exavault.com/developer/api-docs/v2/#section/Obtaining-Your-API-Key-and-Access-Token) for the step-by-step guide to create your key and/or token.  

If you are not an admin-level user of an ExaVault account, you'll need someone with admin-level access to follow the steps and give you the API key and access token.

**Step 3 - Add Your API Credentials to the sample code**

Before you can make an API call, you'll need to make a copy of the environment file provided with this code library. In that same directory where you ran `composer install` above, do:

```bash
% cp .env.example .env
```

Now that you have a file named .env, you need to add in your API credentials. Edit the .env file you just created.

- replace **your\_key\_here** with your API key. Don't add any extra spaces or punctuation
- replace **your\_token\_here** with your access token.
- replace **your\_account_name** with the name of your ExaVault account

And save the file.

**Step 4 - Run the sample script**

Now you're ready to run your first sample. Try sample_get_account_info first

```bash
% php sample_get_account_info.php
```
If everything worked, the sample code will run and connect to your account. You'll see output similar to this:

```bash
% php sample_get_account_info.php
Account used: 40GB (11.4%)
Total size: 350GB
```

## Running Other Sample Files

There are several other sample files that you can now run. You won't need to repeat the steps to set up the .env file each time - the same environment information is used for all of the sample scripts.
Some of the sample scripts will make changes to your account (uploading, creating shares or notifications, etc). Those are marked with an asterisk below:

Script                        | Purpose    \*=Makes changes to your account when run                                   | APIs Used                      |
------------------------------|----------------------------------------------------------------------------------------|--------------------------------|
sample_get_account_info.php   | List the amount of available space for your account                                    | AccountApi                     |
sample_add_notifications.php  | Add upload and download notifications<br/>_\*adds folders to your account_             | ResourcesApi, NotificationsApi |
sample_add_user.php           | Add a new user with a home directory <br/>_\*adds a user and a folder to your account_ | UsersApi                       |
sample_compress_files.php     | Compress several files into a zip file <br/>_\*adds files and folders to your account_ | ResourcesApi                   |
sample_download_csv_files.php | Search for files matching a certain extension, then download them.                     | ResourcesApi                   |
sample_get_failed_logins.php  | List usernames who had a failed login in the last 24 hours                             | ActivityApi                    |
sample_list_users.php         | Generate a report of users in your account                                             | UsersApi                       |
sample_shared_folder.php      | Create a new shared folder with a password<br />_\*adds a folder to your account_      | ResourcesApi, SharesApi        |
sample_upload_files.php       | Upload a file to your account.<br />_\*uploads sample PDFS to your account_            | ResourcesApi                   |

## If Something Goes Wrong

**Problem - Failed to open stream: No such file or directory**

If running the sample script returns a PHP warning about your autoload file like the one shown below, you need to run the `composer install` command as described under **Step 1 - Install dependencies** 

```bash
PHP Warning:  require_once(/samples/vendor/autoload.php): failed to open stream: No such file or directory in /samples/sample_get_account_info.php on line 2
```

**Problem - PHP Fatal Error**

When you try to run a sample, you may see an error like this:

```bash
PHP Fatal error:  Uncaught Dotenv\Exception\InvalidPathException: Unable to read any of the environment file(s) at
```

This error indicates there is a problem with the environment file. Make sure you have a file named .env in the same directory as the sample scripts, and that it contains your API key, access token, and your correct account name.

**Problem - 401 Unauthorized Response**

If running the sample script returns a 401 Unauthorized error like the one shown below, there is a problem with your API credentials. Double-check that the .env file exists and contains the correct values. If all else fails, you may need to log into the ExaVault web file manager and re-issue your access token.

```bash
Exception when calling AccountApi->getAccount: [401] Client error: `GET https://exavaultsupport.exavault.com/api/v2/account` resulted in a `401 Unauthorized` response:
{"responseStatus":401,"errors":[{"code":"ERROR_INVALID_CREDENTIALS","detail":"HTTP_UNAUTHORIZED"}]}
```

**Other problems with sample code**

If you encounter any other issues running this sample code, you can contact ExaVault support for help at support@exavault.com.

## Writing Your Own Code 

When you're ready to write your own code, you can use our sample code as examples. You'll need to:

1. Install our code library using `composer require exavault/evapi-php`
1. Add the line ```require_once(__DIR__ . '/vendor/autoload.php');``` to the top of your script
1. You can use an .env file just like our sample scripts do, or just set variables within your scripts for your API key and access token
1. Whenever you instantiate an Api object (ResourcesApi, UsersApi, etc.), override the configuration to point the code at the correct API URL:
```php
$account_url = "https://YOUR_ACCOUNT_NAME_HERE.exavault.com/api/v2/";
$accountApi = new Swagger\Client\Api\AccountApi(
    new GuzzleHttp\Client(),
    (new Swagger\Client\Configuration())->setHost($account_url)
);
```
```php
$resourcesApi = new Swagger\Client\Api\ResourcesApi(
    new GuzzleHttp\Client(),
    (new Swagger\Client\Configuration())->setHost($account_url)
);
```
```php
$usersApi = new Swagger\Client\Api\UsersApi(
    new GuzzleHttp\Client(),
    (new Swagger\Client\Configuration())->setHost($account_url)
);
```

## Author

support@exavault.com

