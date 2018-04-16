# The Ark Command Line Client (ark-client)

Most of you already know the Ark Desktop Application.

<https://github.com/ArkEcosystem/ark-desktop/releases>

It is an easy to use application to manage your wallets. For some use cases it
makes sense to use the command line client to interact with the Ark blockchain
instead. This article will provide a short introduction to get you going with
the “Ark Command Line Client (ark-client)” on a windows machine.

#### Install Preconditions

In order to use the command line you will need **npm (node pagacke manager).**
It is used to install and run the command line client. Luckily **npm **is
included in the node.js installer which has an easy to use Windows Installer
(msi). You can download and use the node.js installer from this website:

<https://nodejs.org/en/download/>

![1](/img/ark-command-line-client/1_dG40qPkUJu-qDlvKnjI0eg.png)

You can follow the installation wizard to install node.js and npm. Once that is
done we can do everything else in the command line.

#### Open Powershell

Powershell (PS) is a command line tool of windows which we will use for this
guide to work with the “Ark Command Line Client”. Just click start and type
“Powershell”. It should come up as top search result.

![2](/img/ark-command-line-client/1_i4y9UZWsKtEbKz0WoF5TiA.png)

The powershell will look like that:

![3](/img/ark-command-line-client/1_Xr4n60ZUAI4wVUPvxpDrhg.png)

Now we are ready to install the “Ark Command Line Client”.

#### Install Ark Command Line Client

The installation is explained on the github page:

<https://github.com/arkecosystem/ark-client>

All you need to do is to copy/paste that line into the Powershell:


![4](/img/ark-command-line-client/1_xhKa-PlKDaB0cwF4iHT5Ew.png)

I already had it installed. Thus my output is a bit shorter than a new install.
You will see a bit more text. But now you are ready to launch the tool. I will
now switch to call the “Ark Command Line Client” by it’s technical name
(ark-client) to make it a bit shorter.

#### Start the ark-client

We installed it globally. This means you can just type the following line in the
powershell in any directory and it will start the ark-client.

    ark-client

![5](/img/ark-command-line-client/1_bpINwx0RSiYuQYpfuImQLA.png)

Now the ark-client is started and you can interact with it by entering commands.

#### Using the ark-client

The first thing you will want to do is check all available commands.

    help

![6](/img/ark-command-line-client/1_2A1otbPLOPrUr7oAHaFx6g.png)

Let’s have a look at all the available commands:


If you want to use one of these commands you will have to type it and replace
the complete part in the brackets (including the brackets). The first thing you
have to do is to connect to a network. Let’s use devnet for now in order to test
and learn. Devnet is a clone of Ark mainnet for testing purposes. It is ideal as
you don’t have to worry about loosing any Ark.

    connect devnet

![7](/img/ark-command-line-client/1_xvLALLFH_m1l8kvkj0eJEg.png)

Now we can create an account:

    account create

![8](/img/ark-command-line-client/1_eKo6r-A7X5R6idEr4s4k9g.png)

Make sure you write down your passphrase and address. Once you get some Dark on
your wallet you will have to enter your passphrase to create transactions. We
call the token on the devnet “Dark” to distinguish it from the “Ark” on the
mainnet. Devnet addresses start with a “D” and mainnet addresses with an “A”.
You can get devnet tokens for free if you ask in the slack channel #devnet by
providing your devnet address.

<https://ark.io/slack>

I am sure you can find out the rest from here.

**If you connect to mainnet you will be interacting with the real Ark blockchain
and your precious Ark. So be careful what you do!**

#### Conclusion

I hope that gave a short introduction into an alternative client to use Ark. You
will want to use the Ark Desktop most of the time but it is good to know that
there is an other option.

Delegate jarunik<br> [https://arkcoin.net/](https://arkcoin.net/)
