# Economic Attack Vectors in Smart Contracts  

## Introduction  

This repository serves to contain the fundamentals of attacks on smart contracts which have an economic component. The field of smart contract security is still growing and only by identifying and mastering the fundamentals could we safegaurd DeFi.  

The Turing-completeness of Solidity allows for almost any program to be built on top of an EVM blockchain. These applications can be anything from simple tokens to DAOs. Smart contracts and economics are intertwined at the code-level.  

### A note to readers:
**This work is still in it's early stages and is very far from complete. To keep it simple, I try to write from memory where possible (trying to ELI5).** 

You are welcome to ask questions about any of the material in this repo. If you spot a mistake or feel that I haven't attributed a source, feel free to reach out, you can find me at @lourenslinde on twitter. This is a repository of my own knowledge, which is not static. Smart contracts and the microeconomic models built into the code are incredibly fascinating and the thoughts contained herein are likely to evolve as my understanding of the subject matter expands. 

## Method
The approach will be to review the current economic fundamentals in use in various protocols, then we will isolate the generalized economic attack vectors which are used to target smart contracts. From generalized vectors we will dig into specific examples of vulnerabilities and how the intended economics and the implemented code combined to create the vulnerability. But, the approach is to apply first principles thinking by keeping explanations simple.


## Outline  
Most econonmic vulnerabilities arise from code errors introduced when a developer tries translate an economic objective into the smart contract. Market design and token economics go hand in hand. There is an art to translating economics (or any other abstract idea) from paper to code; this art is where Solidity developers thrive. But artists strive toward perfection and yet rarely achieve it. We can assume developers (like me) while striving for perfection, will also fall short at times. Although this repo will not focus on market and token economic design specifically, it may investigate cases where such design in and of itself led to vulnerabilities.  

We start our journey by investigating the basic financial primitives decentralized finance is built on and we expand on the topic as we dig deeper into the code.  

> Writing helps me to clarify my thinking. This repo is a way to maintain a grip on the fundamentals as I research, audit and reflect. As I gain experience I might add/rewrite/remove entire sections. 

1. Market Makers  
2. Lending  
3. Perpetuals  
4. Stablecoins  
5. Oracles  
