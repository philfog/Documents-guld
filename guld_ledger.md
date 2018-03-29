

![guldlogo](https://github.com/Alexstang/branding/blob/master/Guld%20Logo.png)


# guld ledger


__Authors:__

Ira Miller <public@iramiller.com>

Cindy Zimmerman <cindy@tigoctm.com>

License: CC-BY-4


DRAFT

v0.0.1



## Overview


Guld uses ledger-cli as the official ledger format and processing engine. This document describes the specific format which is used in the parent guld group’s ledger, and is not meant to preclude alternate structures in child groups.

## Accounting Principles
Account 1 is always a top level guld name. 

Account 2 is one of the standard accounting categories: Assets, Liabilities, Income, Expenses, Equity.

Accounts 3 and onward are for subdividing the categories.

Assets are only spendable with the signature(s) of the key(s) belonging to the account 1 identity. Equity is never transferrable, but gives weight to votes by the account 1 identity in the name of an account 3 identity.
## Transactions

### Grant

; requires majority of guld equity to approve

2017/12/01 * Grant for work done

    guld:Liabilities   -x guld
   
    guld:Equity:Receiver   x guld
   
    Receiver:Assets   x guld
   
    Receiver:Income   -x guld
   
### Registration

; requires only sufficient guld and an open name

2017/12/01 * individual registration

    Username:Assets   -1 guld
    
    Username:Expenses:guld:register   1 guld
    
    guld:Liabilities   1 guld
    
    guld:Income:register:individual:Username   -1 guld


; m = number of members

2017/12/01 * group registration

    Username:Assets   -m guld
    
    Username:Expenses:guld:register   m guld
    
    guld:Liabilities   m guld
    
    guld:Income:register:group:groupname   -m guld


2017/12/01 * device registration

    Username:Assets   -0.1 guld
    
    Username:Expenses:guld:register   0.1 guld
    
    guld:Liabilities   0.1 guld
    
    guld:Income:register:device:Username:devicename   -0.1 guld
    
### Transfer

; requires signature of the debtor

2017/12/01 * transfer

    Sender:Assets   -x guld
    
    Sender:Expenses   x guld
    
    Receiver:Assets   x guld
    
    Receiver:Income   -x guld
    


TODO file names and locations
