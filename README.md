# Meta Learning (few-shot-learning) explained
> An Introduction and exploration of Meta learning architucture specifically **few-shot-learning**.

## Tables of contents
1. [Motivation for Meta learning](#motivation-for-meta-learning)
1. [General idea about few-shot-learning](./)
1. [Defining common terms for few-shot-learning](./)
1. [Diffenrent techniques for training the **similarity function**](./)
1. [Contacts](./)
1. [Rsources](#resources)
1. [License](#license)

## Motivation for Meta learning
> Learning visual models of object categories requires thousands of training examples; this is due to the diversity and richness of object appearance which requires models containing hundreds of parameters.
- Informal observation tells us that Humans can learn a new category of animals both fast and easy
- Human can learn to recognize an animal that they have never seen before by seeing 2 or 3 images of it.
- ***In The other side:*** Computer requires thousands if not tens of thousands of training images to be able to reconize a new category.
- Could computer vision algorithms be similarly efficient?
- One possible explanation of human efficiency is that when learning a new category we take advantage of prior experience.
  - The appearance of the categories we know and, more importantly, the variability in their appearance, gives us important information on what to expect in a new category. This may allow us to learn new categories from few(er) training examples.
- And here we will try to explain how we can **some how** give the copmuter the ability to mimic what Humans do.




## Contacts
> You can reach out for me in twitter @amshrbo[twitter.com/amshrbo] or via mail `amshrbo@gmail.com`

## Resources
- You can Watch **Shusen Wang** videos on [YouTube](https://www.youtube.com/playlist?list=PLgtf4d9zHHO8YjSSkkBT55XN8xsIvb-ku).
  - You can find the slides associated with videos [here](https://github.com/wangshusen/DeepLearning) Just go to this repos and search for Meta Learning.
- [One-Shot Learning of Object Categories](https://www.computer.org/csdl/journal/tp/2006/04/i0594/13rRUxC0SXe) Lei Fei-Fei, Fergus, Perona, IEE, 2006

## License
[GPL-3.0 License](./LICENSE)
