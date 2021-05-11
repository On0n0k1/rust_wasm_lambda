# RustWasmLambda

Implementation of lambda functions with serverless framework through webassembly in rust. We have AWS lambda crud functions using wasm, npm, rust and serverless framework.

# Intentions

This project is built to be cloned by anyone willing to build simple compiled lambda applications written in rust.

# Credits

Special thanks to Colin Eberhardt for the blog post that teaches how to do it. 
[Source](https://blog.scottlogic.com/2018/10/18/serverless-rust.html)

# How to use it

## Install Git

For cloning this and using wasm-pack. [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## Install Rust

[(https://www.rust-lang.org/learn/get-started)](https://www.rust-lang.org/learn/get-started) 


## Install NodeJS NPM

[https://docs.npmjs.com/downloading-and-installing-node-js-and-npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)


## Install Docker 

Used by serverless to invoke local functions. Local functions are for testing. [https://www.docker.com/get-started](https://www.docker.com/get-started)

## Install Wasm-Pack

Used to compile Rust into javascript webassembly. [https://rustwasm.github.io/wasm-pack/installer/](https://rustwasm.github.io/wasm-pack/installer/)

## Install libssl-dev

I found out that rust won't compile wasm in my ubuntu 20.04 without it (There will be a message saying that it's missing). Here is a link for it: [https://zoomadmin.com/HowToInstall/UbuntuPackage/libssl-dev](https://zoomadmin.com/HowToInstall/UbuntuPackage/libssl-dev).

## Install AWS-CLI

Used by serverless. [https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

## Install Serverless

Simplify a lot of cloudformation implementation. Supports other platforms besides aws like firebase.
[https://www.serverless.com/framework/docs/getting-started/](https://www.serverless.com/framework/docs/getting-started/)

## Create an AMI user account in AWS

Serverless will connect through that account. If you work with aws services you know what I mean. If you don't, I recommend googling and searching around how to set it up. I'm not going through that headache trying to find all the links again. Anyway, here's a summary.

When we create an aws account, we login through the root. But doing that is unsafe, because we don't want to risk losing that password with admin permissions for everything in our account. So we can make other accounts with limited access to the account's resources. Therefore we can have one account for the company/admin, and several tiny accounts that can access the services like lambda, database, etc.

AMI is tiny account that is part of the bigger account. Now let's say I created an IAM account and gave it admin permissions (I don't know which permissions serverless require for the lambda access). I then need to download the csv file with the access keys. If we lose the csv file, we need to create another one (I think), so store it properly.

We need the ***access key ID*** and ***Secret Access Key*** to connect serverless to your account.

No one has permission to call the lambda functions without explicitly saying it in  the yml file. So to access your lambda function just write it in the serverless.yml file... I don't know how to do it yet.

Functions can still be tested locally, though.

# How to set up everything

 - cd into the file where this project will be

 - run: git clone https://github.com/On0n0k1/rust_wasm_lambda.git
 
 - run: wasm-pack build --target nodejs

 - run: aws configure

 Add the ***access key ID*** and ***Secret Access Key*** referred above. You can also add region, I put myne as "sa-east-1" (South America east 1). Pressing enter will set to use default.

  - run: serverless invoke local --function ***$functionname***

Replace ***$functionname*** with the function name in the serverless file. Currently the only function is hello.

## If last command doesn't work

If aws configure + serverless invoke doesn't work. Creating a named profile might work. Here's a better summarized tutorial: [https://medium.com/ivymobility-developers/configure-named-aws-profile-usage-in-applications-aws-cli-60ea7f6f7b40](https://medium.com/ivymobility-developers/configure-named-aws-profile-usage-in-applications-aws-cli-60ea7f6f7b40).

In summary, considering we want to create a profile named "serverless":

 - run: aws configure --profile serverless

Add the ***access key ID*** and ***Secret Access Key*** referred above. You can also add region, I put myne as "sa-east-1" (South America east 1). Pressing enter will set to use default.

 - run: serverless invoke local --function ***$functionname*** --aws-profile serverless

Replace ***$functionname*** with the function name in the serverless file. Currently the only function is hello.


# How to remove it

Remove the lambda from your serverless and aws accounts with this.

 - run: serverless remove

