# Accounts

Liferay Commerce stores information about customers in an account. Accounts
primarily store shipping and billing addresses and payement information, but
can be used to store other kinds of information too.

In Liferay Commerce, a user must be assigned to an account in order to make a
purchase.

Accounts are managed and users assigned to them using the Accounts widget.

## Account Types

There are two types of accounts: business and personal.

A **personal account** stores information about an individual user. That user
is the only user who can be assigned to the account. When you assign a user to
an account, information from that user's profile is copied to the account.

A **business account** stores information about a business. Any number of users
can be assigned to a business account, and each can be assigned roles that
correspond to their position in that business. User profile data is not copied
to the account.

## Account Roles

A **role** is a group of permissions collected around a particular purpose. An
**Account Role** consists of permissions related to editing an account or
managing an account's orders. You can create your own account roles, but the
basics are included:

Account Administrator: Account admins can do anything. They can modify the
account, invite users to join the account, and assign roles to other members.
They also have all the other permissions that other account roles have.

Buyer: Buyers can view, create, and check out orders.

Order Manager: have all the permissions of buyers, and can also manage, delete
and approve orders.

A typical approach would be to enable a workflow on your site such that Account
Administrators assign the roles, Buyers create orders, and Order Managers
approve them. But you can customize the roles any way you want.

## Seller-side Account Management

Accounts can also be managed by Administrators and Sales Agents. The Sales
Agent is a role that allows a user to manage any account assigned to him
without granting him administratrative permissions.

To give a sales agent access to accounts:

1. Group your accounts in organizations using the Accounts Widget.

2. Assign sales agents to the same organizations.

Sales agents can access any account within the organization to which she is
assigned.
