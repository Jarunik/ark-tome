# The Ark Blockchain Database

Until recently I did not have much clue about the postgres database on which the
Ark blockchain is stored. That gave me the idea to have a closer look and
document the process. Feel free to follow along. In order to start we need an
Ark relay node on ubuntu to access the database. You can follow one of my guides
to get that setup.

<https://medium.com/@jarunik/installing-and-managing-an-ark-node-577a9074b9bb>

Now you should have an Ark node an your Ubuntu up and running and we can get
going. Please login to your server and then we will login to the database.

### Database Command Line Interface

Let’s login to postgres.


The command prompt will change and will look like that.

    postgres=#

If we want to get back out we can do that easily.

    \q

But let’s stay and have a look what is on that database.

    \l

You can quit the report with q.

    q

You should find a database named “ark_mainnet” which is owned by your user (if
you installed the ark-node correctly). That is where the Ark blockchain is
stored. Let’s have a closer look.

### Query some simple Data

So let’s try to list all tables in our database containing any data. First we
have to connect to the right database.


The prompt will change as you are connected to the ark_mainnet database now. It
will looks like that:

    ark_mainnet=#

Then we can list all tables of the database. That is where the Ark blockchain is
stored.

    \dt

                         List of relations
     Schema |              Name              | Type  |  Owner
    --------+--------------------------------+-------+---------
     public | blocks                         | table | jarunik
     public | delegates                      | table | jarunik
     public | forks_stat                     | table | jarunik
     public | mem_accounts                   | table | jarunik
     public | mem_accounts2delegates         | table | jarunik
     public | mem_accounts2multisignatures   | table | jarunik
     public | mem_accounts2u_delegates       | table | jarunik
     public | mem_accounts2u_multisignatures | table | jarunik
     public | mem_delegates                  | table | jarunik
     public | migrations                     | table | jarunik
     public | multisignatures                | table | jarunik
     public | peers                          | table | jarunik
     public | signatures                     | table | jarunik
     public | transactions                   | table | jarunik
     public | votes                          | table | jarunik
    (15 rows)

I guess most of the tables seems easy to explain. But let’s try to select
something. Let’s get the height of the blockchain.

    SELECT MAX(height) FROM blocks;

We can get the current height of the blockchain on your node. That wasn’t too
difficult.

### Ark database tables

Let’s try to explain what each table is about. I guess the node code itself will
also help to understand the different tables a bit better.

<https://github.com/ArkEcosystem/ark-node/tree/development/sql>

But let’s just look at the database in this article.

#### blocks

Let’s see what columns the table offers.

    \d+ blocks

Now we got all columns of that table. Each row in this table represents a block.
We can get the height of a certain block.

    SELECT "height" FROM blocks where "id" = '16862733788990232613';

#### delegates

Seems to host all delegates. Let’s repeat looking at columns one last time. I am
sure you can do it yourself going forward.

    \d+ delegates

Let’s try to get my delegate.

    SELECT * FROM delegates WHERE username = 'jarunik';

Unfortunately there is not much to see. We just get a transaction id. It is the
delegate registration transaction. So this table is storing delegate
registrations. Here is my delegate registration on the explorer:

#### forks_stat

This tables seems to list all forks of a certain delegates.

    SELECT COUNT(*) FROM forks_stat;

     count
    -------
         0
    (1 row)

Looks like this table is not used anymore by the current ark-node code. Most
likely deprecated.

#### mem_accounts

Looks like all accounts in the database.

    SELECT balance*0.00000001 balance FROM mem_accounts WHERE username = 'jarunik';

       balance
    --------------
     486.30259220
    (1 row)

Seems about right.

    SELECT COUNT(*) from mem_accounts;
     count
    -------
     35031
    (1 row)

Ark has currently 35031 accounts.

#### mem_accounts2delegates

This is the relation between accounts and delegates. It lists which accounts
votes for which delegate.

#### mem_accounts2multisignatures

This is the relation between accounts and multisignatures.

#### mem_accounts2u_delegates

The 2u tables seem to be temporary tables for updates. The information is first
stored in them and then updated in the main table with a delete and insert.

#### mem_accounts2u_multisignatures

The 2u tables seem to be temporary tables for updates. The information is first
stored in them and then updated in the main table with a delete and insert.

#### mem_delegates

This is the table which has finally some more information about the delegates.

    SELECT COUNT(*) FROM mem_delegates WHERE "publicKey" = '02c7455bebeadde04728441e0f57f82f972155c088252bf7c1365eb0dc84fbf5de';
     count
    -------
     39714
    (1 row)

You can find all blocks produced by the delegate. Jarunik already created more
than 39k blocks.

You can also just look at one round.

    SELECT * FROM mem_delegates WHERE "publicKey" = '02c7455bebeadde04728441e0f57f82f972155c088252bf7c1365eb0dc84fbf5de' LIMIT 1;

#### migrations

Ark seems to have currently 4 migrations.

    SELECT COUNT(*) FROM migrations;

     count
    -------
         4
    (1 row)

But they are just the creation of the database:

    SELECT * FROM migrations;

           id       |           name
    ----------------+---------------------------
     20160723182900 | createSchema
     20160723182901 | createViews
     20160724114255 | createMemoryTables
     20160908215531 | protectMemAccountsColumns
    (4 rows)

#### multisingatures

The columns for multisignatures are:

* min
* lifetime
* keysgroup
* transactionId

    SELECT COUNT(*) FROM multisignatures;
     
    count
    -------
         2
    (1 row)

They are not really used much yet. I guess we can skip looking at them for now.

#### peers

Looks like we do not store the peers in the database anymore.

    SELECT COUNT(*) FROM peers;

     count
    -------
         0
    (1 row)

Seems deprecated.

#### signatures

It is a pair of transaction and public keys.

    SELECT COUNT(*) FROM signatures;

     count
    -------
      1099
    (1 row)

When checking a transaction in the explorer we see that it is second signature
creations of accounts. The query gets 10 entries of the table:

    SELECT * FROM signatures LIMIT 10;

There are currently 1099 second signatures.

#### transactions

I guess that is where all the magic happens. All transactions in a nice list.

    SELECT COUNT(*) FROM transactions;

     count
    --------
     337975
    (1 row)

There are already 337k transactions executed since the start.

    SELECT SUM(fee)*0.00000001 as fee from transactions;

          fee
    ----------------
     73301.60000000
    (1 row)

All these transactions costed 73k Ark in fees which have been collected (and
shared) by the delegates.

#### votes

These are all vote transactions containing the information who was voted.

    SELECT COUNT(*) FROM votes;

     count
    -------
     23256
    (1 row)

There is quite a lot of voting happening (23k).

### Conclusion

I hope that gave you a little overview of the database of the Ark blockchain. It
is pretty simple. It is just a short list of tables which in essence contain the
following information:

* blocks
* delegates
* accounts
* transactions

Thanks for reading.

Delegate Jarunik<br> [https://arkcoin.net/](https://arkcoin.net/)