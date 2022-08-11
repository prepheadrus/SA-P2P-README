# SUPERALGOS OUTGOING AND INCOMING SIGNALS


![image](https://user-images.githubusercontent.com/38046064/183960313-a0bef31a-1779-41be-b55f-cb8e303e70ae.png)


Table of Content:

- Getting Ready
- Set up your [User Profile](#set-up-your-user-profile)
- Set up Superalgos [P2P Environment](#note)
- Set up a Workspace for [SENDING](#outgoing-signals) Signals
- Set up a Workspace for [RECEIVING](#incoming-signals) Signals
- [Troubleshoots](#troubleshooting-errors)

---

## GETTING READY

This Readme covers the set up of Superalgos in order to be able to send or receive *signals* from the Superalgos P2P Network.

It's supposed that you're familiar with Superalgos. But if you don't feel ready about it, follow the instructions in the main README file for developers and contributors. 

You can get help from videos about installation https://youtu.be/Q4HVdfNdHbk or how to make a User Profile https://youtu.be/Sa5B-bwg81A.

Whether you want to be a *Sender* or a *Receiver* there are few concepts to keep in mind:

- The **User Profile** that becames essential to be able to identify users in the P2P network (i.e. A Sender can decide to send signals only to specific users. And a Receiver can be sure that signals are sent from a specific user and nobody else)

- The **P2P Network** that is necessary for the *Senders* but NOT for *Receivers*!




## SET UP YOUR USER PROFILE

![image](https://user-images.githubusercontent.com/38046064/183973288-81b2ccd3-f36e-41b2-92e3-6a0f3c683d9e.png)


The User Profile setup of a Sender is only slightly different from a Receiver. 

### FOR RECEIVERS

Set up your User Profile adding these nodes:

- User Apps > Server Apps > Task Server
- User Apps > Server Apps > Social Trading Server
- User Bots > Social Trading Bots > Social Trading Bot > Available Signals
- P2P Network Nodes > P2P Network Node

If you just want to receive signals, then you do not need to run any servers. But in order to be able to connect to the signals provider (i.e. a Superalgos User) the following nodes must be added to your User Profile: 

Under the P2P Network Node in your user profile 
- Add Network Services > Trading Signals
- Reference P2P Network Reference to the network of choice (mainnet or testnet for public signals or create your own permissioned P2P network)

#### FOR SENDERS 

All the above plus:

- User Bots > Social Trading Bots > Social Trading Bot > Available Signals > Trading System Signals > Trading Strategy Signals
- User Bots > Social Trading Bots > Social Trading Bot > Available Storage
- User Storage > Github Storage > Github Storage Container

N.B. Under the Social Trading Bot node in your profile, you must
- Reference Available Storage Reference to the Github Storage Container of choice
- Edit/check Trading Strategy Signals. Here you setup which signals you want to send. More details are found under the section "Trading System"


### Github Storage Container

In order to be able to send signals, *Senders* must have a Storage Container. In this current development Superalgos makes use of Github repositories that will host the files, don't worry they are encrypted :)

Under the Github Storage Container
- Edit codeName to a unique identifier
- Edit githubUserName to your github user account
- Edit repositoryName to your github repository where you'll save the trading signals

You must also add a JSON-file to your Superalgos folder, under Superalgos/My-Secrets create a file called "ApisSecrets.json", enter the following details in the following format.

```
{
	"secrets": [
		{
			"nodeCodeName": "codeName-that-you-have-chosen-in-github-storage-container",
			"apiToken": "your-api-token-from-github"
		},
		{
			"nodeCodeName": "codeName-that-you-have-chosen-in-github-storage-container-number2",
			"apiToken": "your-api-token-from-github"
		}
		]
}
```	


### Signing Account 

After you are done with your changes to your user profile, you must sign them. This is to ensure that you are you and noone else. This is done with the Profile Constructor (poart of the Governance project). 

- Reference your User Profile to the profile constructor
- Profile contructor > Installing signing account
- User Profile > Save Plugin
- **PR your updated profile**

Remember to save your User Profile plugin, contribute it and check that it was merged at the Governance repository.

**IMPORTANT**: It takes a few minutes for your profile to be auto-merged into the Governance repository and another 5 minutes to be picked up by the running Network Node. After changes to your profile, wait for around 10 minutes before expecting it to be able to connect to the Superalgos Network node.



## SUPERALGOS P2P ENVIRONMENT

Whatever network you choose to send signals over, the Environment.js file (located in under Superalgos folder), has to be changed to the same network type and network codename you want to use.

The following was used for Permissioned P2P Network.

```
DESKTOP_TARGET_NETWORK_TYPE: 'Permissioned P2P Network',
DESKTOP_TARGET_NETWORK_CODENAME: 'BlaaSignals',
TASK_SERVER_TARGET_NETWORK_TYPE: 'Permissioned P2P Network',
TASK_SERVER_TARGET_NETWORK_CODENAME: 'BlaaSignals',
```


**Using Tesnet will at the moment result in interference with the Machine Learning Project, as they are using Testnet. Should not happen, but seems to be a bug there.**


	
**NOTE:** There needs to be network nodes running for the choosen network, if permissioned p2p you need to run your own node. 



	
---
## OUTGOING SIGNALS

To be able to send signals using the built in features of Superalgos, you must first prepare your User Profile with specific nodes. You will also need a dedicated Github Repo as well as trading system and workspace. 





### Trading System

Next step is to setup the trading system! 

- Reference Trading System Outgoing Signal Reference to Trading System Signal (Under Socical Trading Bot > Available Signals)
- Add outgoing signals for choice, you can add as many or as few that you wish
	- If signals are based on formulas, you can if you wish add Signal Context Formula, to even further edit the value being sent. 
- Reference all outgoing signals to the correct Signal under Trading Strategy Signals.
	- For instance, a Market Buy Signal in the trading system goes to the Market Buy Signal under Trading Strategy Signals. 

Signals cannot be used as a conditions on it's own, to use signals as a true/false statement, enter the following as a conditions to the event: 

```
if (signals !== undefined && signals.length > 0) {
    true
} else {
    false
}
```

### P2P Network Node

Almost there, you need to add a couple of nodes before you run your trading task (either Testing Trading Tasks or Production Trading Tasks).

- Add Task Server Reference on Task
- Reference Task Server Reference to the a free Task Server in the User Profile 
- Add Social Trading Bot Reference to Trading Bot Instance
- Reference Social Trading Bot Reference to the correct Social Trading Bot in User Profile (the one that will send your signals)


---

# INCOMING SINGALS 

Set up a Workspace for REIVING signals from a Superalgos User.
## User profile
- Add User Apps > Server Apps > Task Server
- Add User Bots > Social Trading Bots > Social Trading Bot > Available Signals > Incoming Signals

## Incoming Signals

Here you add what signals you want to use in your trading system. 
You reference them from the user profile that is sending the signals (under available signals > trading system signals > trading strategy signals)
Note: You have to add the trading system signal, otherwise the candles won't sync. 


## Signing Account

Reference your User Profile to the profile constructor
Profile contructor > Installing signing account

-PR your updated profile


## P2P Network Node (Either Testing Trading Tasks or Production Trading Tasks)

Add Task Server Reference on Task
Reference Task Server Reference to the correct Task Server in the User Profile
Add Social Trading Bot Reference to Trading Bot Instance
Reference Social Trading Bot Reference to the correct Social Trading Bot in User Profile




---

# TROUBLESHOOTING ERRORS

### Network Client Identity

"Fatal Error. Can not run this task. The Network Client Identity does not match any node at User Profiles Plugins."
This error occurs when the signing account does not match the Governance plugin repository's account. To ensure they are the same, import your user profile on the workspace using the "Add specified User Profile" command under Plugins -> Plugin Project -> Plugin User Profiles. Add the correct nodes, references and signing account to the plugin as detailed in App Setup. Save the plugin and push the changes to the Governance repository and wait 10 minutes for it to merge and be picked up by the Forecast Server.