## Quick Start

### Installation

Adldap2 requires [Composer](https://getcomposer.org/) for installation.

Once you have composer installed, insert Adldap2 into your `composer.json` file located in the root of your project directory:

```
"require": {
    "adldap2/adldap2": "8.0.*"
},
```

Run `composer update` in the root of your project.

You're all set!

### Usage

```php
// Construct new Adldap instance.
$ad = new \Adldap\Adldap();

// Create a configuration array.
$config = [
  // Your account suffix, for example: jdoe@corp.acme.org
  'account_suffix'        => '@corp.acme.org',
  
  // The domain controllers option is an array of your LDAP hosts. You can
  // use the either the host name or the IP address of your host.
  'domain_controllers'    => ['ACME-DC01.corp.acme.org', '192.168.1.1'],
  
  // The base distinguished name of your domain.
  'base_dn'               => 'dc=corp,dc=acme,dc=org',
  
  // The account to use for querying / modifying LDAP records. This
  // does not need to be an actual admin account.
  'admin_username'        => 'admin',
  'admin_password'        => 'password',
];

// Add a connection provider to Adldap.
$ad->addProvider($config);

try {
    // If a successful connection is made to your server, the provider will be returned.
    $provider = $ad->connect();

    // Performing a query.
    $results = $provider->search()->where('cn', '=', 'John Doe')->get();
    
    // Finding a record.
    $user = $provider->search()->find('jdoe');

    // Creating a new LDAP entry. You can pass in attributes into the make methods.
    $user =  $provider->make()->user([
        'cn'          => 'John Doe',
        'title'       => 'Accountant',
        'description' => 'User Account',
    ]);

    // Setting a model's attribute.
    $user->cn = 'John Doe';

    // Saving the changes to your LDAP server.
    if ($user->save()) {
        // User was saved!
    }
} catch (\Adldap\Auth\BindException $e) {

    // There was an issue binding / connecting to the server.

}
```
