## Welcome to the DeepRacer Wiki

This wiki is designed to consolidate general information about the [AWS DeepRacer](https://aws.amazon.com/deepracer/) into a cohesive wiki.


### What is the AWS DeepRacer?

According the AWS website:
> AWS DeepRacer is the fastest way to get rolling with machine learning, literally. Get hands-on with a fully autonomous 1/18th scale race car driven by reinforcement learning, 3D racing simulator, and global racing league.

Essentially, the DeepRacer is a platform to learn and refine Artificial Intelligence (AI) and Machine Learning (ML) techniques on an autonomous car.
Unlike other solutions, such as DonkeyCars, DeepRacers can be fully trained and used in a virtual simulation. You don't even have to buy an actual hardware car. 
This lowers the bar of entry exponetially, and truly opens up DeepRacer to anyone. 

### Contributing

This wiki is maintained by the [DeepRacing Slack Community](https://deepracing.io) and [ARCC](https://arcc.ai). 
This wiki was built using [Docsify](https://docsify.js.org). 

Install npm and use `npm i -g docsify` to install the docsify cli. Then run `docsify serve ./` inside the repository to serve the webpage locally

To write new articles, simply create a new Markdown file in the root directory.

For example, the following page structure:

```
|- README.md
|- outline.md
|- getting-started
   |- README.md
   |- account.md
```

Would correlate with the these matching routes:
``` 
README.md                     => https://domain.com
outline.md                    => https://domain.com/outline
getting-started/README.md     => https://domain.com/getting-started
getting-started/account.md    => https://domain.com/getting-started/account
```
