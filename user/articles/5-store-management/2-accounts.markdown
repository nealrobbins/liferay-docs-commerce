# Accounts

In Liferay Commerce, a user must be assigned to an account in order to make
a purchase.

## Account Types

There are two types of account: business and personal.

A personal account has only one user assigned to it. 

A business account can have multiple users and organizations assigned to it.

## Account Roles

The key advantage using accounts rather than users to make purchases is that
users within an account can be assigned various roles. You can create your own
roles, but the basics are included:

Account Administrator: Account admins can do anything. They can modify the
account, invite users to join the account, and assign roles to other members.
They also have all the other permissions that other account roles have, except
for sales agents.

Buyer: Buyers can view, create, and check out orders

Order Manager: have all the permissions of buyers, and can also manage, delete
and approve orders.

Sales Agent: the sales agent is a Regular Role, not an account role, but is
worth mentioning: this seller-side role has permissions to manage account
organizations as well as to manage accounts to which she is assigned.

A typical approach would be to enable a workflow on your site such that Account Administrators assign the roles, Buyers create orders, and Order Managers approve them. But you can customize the roles any way you want.

## Accounts and Organizations

You can add organizations to an account. When you do so, all of the members of
an organization become members of site [at least, I assume so--I haven't been
able to actually make this happen]
