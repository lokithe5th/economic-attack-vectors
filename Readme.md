# Economic Attack Vectors in Smart Contracts  

## Introduction  

This repository serves to contain the fundamentals of attacks on smart contracts which have an economic component. The field of smart contract security is still growing and only by identifying and mastering the fundamentals could we safegaurd DeFi.  

The Turing-completeness of Solidity allows for almost any program to be built on top of an EVM blockchain. These applications can be anything from simple tokens to DAOs. Smart contracts and economics are intertwined at the code-level.  

### A note to readers:
You are welcome to ask questions about any of the material in this repo. If you spot a mistake or feel that I haven't attributed a source feel free to reach out, you can find me at @lourenslinde on twitter. This is a repository of my own knowledge, which is not static. Smart contracts and the microeconomic models built into the code are incredibly fascinating and the thoughts contained herein are likely to evolve as my understanding of the subject matter expands. 

## Method
The approach will be to review the current economic fundamentals in use in various protocols, then we will isolate the generalized economic attack vectors which are used to target smart contracts. From generalized vectors we will dig into specific examples of vulnerabilities and how the intended economics and the implemented code combined to create the vulnerability. 


## Outline  

Most econonmic vulnerabilities arise from code errors introduced when a developer tries translate an economic objective into the smart contract. Market design and token economics go hand in hand. There is an art to translating economics (or any other abstract idea) from paper to code; this art is where Solidity developers thrive. But artists strive toward perfection and yet rarely achieve it. We can assume developers (like me) while striving for perfection, will also fall short at times. Although this repo will not focus on market and token economic design specifically, it may investigate cases where such design in and of itself led to vulnerabilities.  

We start our journey by investigating the basic financial primitives decentralized finance is built on and we expand on the topic as we dig deeper into the code. The choice to start with a primitive and detail the topic through to an exploit is made to make for less cumbersome reading. The intentional choice is to carry the the thread through from primitive to at least one exploit which is purely the result of a misimplemented primitive. This should allow a reader to get up to speed quickly on a vulnerability, without having to refer to distant sections.  

1. Market Makers  
2. Lending  
3. Perpetuals  
4. Stablecoins  
5. Oracles  

### 1. Market Makers  

#### Order books

#### AMMs  

#### Constant Product AMMs  

#### Constant Sum AMMs  

#### Exploits

### 2. Lending  

#### Over-collateralized loans  

#### Under-collateralized loans  

#### Flashloans  

### Perpetuals  

#### Options and Futures  

#### Exploits

### 3. Stablecoins  

#### Use cases  

#### The Curve Wars  

### 4. ERC20 Tokens  

#### Use cases  

#### Common vulnerabilities  

#### Exploits

## Conclusion  