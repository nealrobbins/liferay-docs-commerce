# Accounts

An account stores information about a customer. Accounts store shipping and
billing addresses and, if applicable, a VAT number.

Accounts are managed and users associated with them using the Accounts widget.

## Account and Site Types

There are two types of accounts: business and personal.

A **personal account** stores information about an individual user. You don't
need to worry about creating or managing personal accounts; they're created
automatically when a user first logs in, using data from her profile.

| **Note:** Data is imported from user's profile to her account only when the
| account is generated. Subsequent changes to either the profile or the account
| are not propagated.

A **business account** stores information about a business. Multiple users can
be associated with a business account, and can be assigned unique [Account
Roles](#account-roles). 

The type of accounts you use are determined by your site type.

### Site Types 

Your site type should match your store's business model: B2B, B2C, or B2X.

**B2B:** A business-to-business site only recognizes business accounts, which
means that a user must be explicitly associated with a business account in order
to make purchases. This is recommended for B2B sellers or B2C sellers whose
customers are grouped into a single buying unit, for example households.

**B2C:** A business-to-consumer site only recognizes personal accounts, which
means that any authenticated user can make purchases. This is recommended for
B2C sellers or B2B sellers who do not care to support multiple users on a single
account.

**B2X:** A B2C-B2B site recognizes both personal and business accounts. Users
can be associated with business accounts, but can also make purchases
individually. This is recommended for sellers who want to support both account
types on the same site.

#### Changing Site Types

To set your site's type, go to *Site Menu* &rarr; *Commerce* &rarr; *Settings*
and select the *Site Type* tab. Select a type from the drop-down menu and click
*Save*.

Changing a site's type changes which accounts appear in its Accounts widget. If
an instance contains business accounts but a site's type is set to B2C, those
accounts still exist in the database but do not appear in the Accounts widget
and are inaccessible to users.

It is best practice to set a site's type as soon as you create it and avoid
changing it in the future.

## Account Roles

A **role** is a group of permissions collected around a particular purpose.
Assign the following **Account Roles** to customers to allow them to manage
their own accounts and orders:

Buyer: can view, create, and check out orders.

Order Manager: have all the permissions of buyers, and can also manage, delete
and approve orders.

Account Administrator: Account admins can do anything. They can modify the
account, invite users to join the account, and assign roles to other members.
They also have all the permissions that other account roles have.

A typical approach would be to enable a workflow on your site such that Account
Administrators assign roles, Buyers create orders, and Order Managers approve
them. But you can customize or create new roles any way you want: see [Roles and
Permissions](/docs/7-2/user/-/knowledge_base/u/roles-and-permissions).

## Seller-side Account Management Roles

Accounts can also be managed by Administrators and Sales Agents. The Sales Agent
is a role that allows a user to manage any account assigned to him without
granting him administrative permissions.

To give a sales agent access to accounts:

1. Group your accounts in organizations using the Accounts Widget.

2. Associate sales agents with the same organizations.

Sales agents can access any account within any of their associated
organizations.
