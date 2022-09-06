---
description: >-
  This page describes how to setup showrunners scaffold / framework which will
  enable you to rapidly create and deploy web3 notifications from backend.
---

# How to Setup Showrunners?

## Prerequisites

_You should have the following installed on your system_

* Docker
* Node JS
* NPM

## Setting Up Showrunners

The initial step in setting up the showrunners is cloning the showrunners-HelloWorld repo

```jsx
git clone https://github.com/ethereum-push-notification-service/epns-showrunners-framework
```

This repository will contain the showrunner framework that you require to instantly build your channels logic.

Now in order to run this showrunner, we will need  **.env** file which will contain your credentials that are required for fetching data from blockchains (for example, Infura).

For creating this `.env` file, simply copy the `.env.sample` file present in the root of the folder and replace the fields with valid credentials for each field.

```
# MAKE A COPY OF THIS AND FILL WITH YOUR CREDENTIALS AND NAME IT .env (Remove .sample Part)

## NORMAL CONFIG
# DEBUG | CAN BE 'debug' or 'prod'
LOG_LEVEL=debug_or_prod # 'debug' or 'prod'

# WEB3 ENDPOINTS | NEED ATLEAST INFURA OR ETHERSCAN OR ALCHEMY, REST CAN BE NULL
INFURA_PROJECT_ID=your_infura_project_id
INFURA_PROJECT_SECRET=your_infura_project_secret

## ADVANCED CONFIG
# WEB3 ENDPOINTS | OPTIONAL IF YOU FILLED INFURA_PROJECT AND INFURA_PROJECT_SECRET, CAN SET IT TO NULL IF YOU WANT TO
ETHERSCAN_API=your_etherscan_api_key_or_null
ALCHEMY_API=your_alchemy_api_key_or_null
```

| Param                   | Valid Values                                  | Comment                                                                                                                                                                                                                       |
| ----------------------- | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LOG\_LEVEL              | **debug** or **prod**                         | Changes the amount of log generated, recommended to use debug until you are sure of the logic.                                                                                                                                |
| INFURA\_PROJECT         | **your\_infura\_project\_id** or **null**     | <p>Head to <a href="https://infura.io/"><strong>infura.io</strong></a> to generate one, required to query data of blockchain. </p><p></p><p>Need atleast one (Infura, Etherscan or Alchemy) to operate correctly.</p>         |
| INFURA\_PROJECT\_SECRET | **your\_infura\_project\_secret** or **null** | <p>Head to <a href="https://infura.io/"><strong>infura.io</strong></a> to generate one, required to query data of blockchain. </p><p></p><p>Need atleast one (Infura, Etherscan or Alchemy) to operate correctly.</p>         |
| ETHERSCAN\_API          | **your\_etherscan\_api\_key** or **null**     | <p>Head to <a href="https://etherscan.io/"><strong>etherscan.io</strong></a> to generate one, required to query data of blockchain. </p><p></p><p>Need atleast one (Infura, Etherscan or Alchemy) to operate correctly.0</p>  |
| ALCHEMY\_API            | **your\_alchemy\_api\_key** or **null**       | <p>Head to <a href="https://www.alchemy.com/"><strong>alchemy.com</strong></a> to generate one, required to query data of blockchain. </p><p></p><p>Need atleast one (Infura, Etherscan or Alchemy) to operate correctly.</p> |

{% hint style="danger" %}
At least INFURA\_PROJECT, INFURA\_PROJECT\_SECRET or ETHERSCAN\_API or ALCHEMY\_API key is required to correctly operate showrunners. Recommended to put all as the data fetching becomes more decentralised.
{% endhint %}

## Running the Showrunners

Now that you have set up the showrunners .env file, you are all set to run the showrunners. To do that first ensure:

* All dependencies are installed, open the terminal (or command prompt), and go to the root of the cloned repo, `npm install` or `yarn install` to install dependencies.

```
npm install
```

* Next, type `docker-compose up` to start running docker image

```bash
docker-compose up
```

* Finally, open another terminal and ensure you are still at the root of the cloned repo and fire `npm start`  or `yarn start`

```bash
npm start
```

If everything is good, this is the output that you should see! Congrats, you just installed Showrunners framework :relaxed::relaxed: running on the port`:5432` :mechanical\_arm:

![](<../../.gitbook/assets/Screen Shot 2022-05-09 at 12.57.40 PM.png>)

{% hint style="info" %}
You might see that showrunners are throwing an error explaining that no channels can be found, a channel is what is needed to run logic controls through showrunners.&#x20;

Let's dive in and see how to create your hello world channel to run on top of the showrunners framework!
{% endhint %}
