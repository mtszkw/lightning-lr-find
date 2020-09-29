# [Lightning LR Find](https://enjoymachinelearning.com/posts/find-lr-pytorch-lightning/)
### Automatically finding good learning rate for your network with PyTorch Lightning

Read full blog post: https://enjoymachinelearning.com/posts/find-lr-pytorch-lightning/  
This project introduces Learning Rate Finder class implemented in PyTorch Lightning and compares results of LR Find and manual tuning.

Among of all hyperparameters used in machine learning, learning rate is probably the very first one you hear about. It may also the one that you start tuning in the first place. You can find the right value with a bit of hyper parameter optimization, running tons of training sessions or you can let tools do it, much faster. Nowadays, many libraries implement LR Finder or “LR Range Test”.

### Example: LR find for Fashion MNIST classification

Basically I wanted to train a fairly simple convolutional neural network (LeNet) on an uncomplicated dataset (Fashion MNIST). I ran four **separate experiments that only differ in initial learning rate** values: 1e-5, 1e-4, 1e-1 and one selected by Learning Rate Finder. It took around 12 seconds to find best initial learning rate which turned out to be **0.0363**.

Looking at loss/LR plot (Figure 1) I was surprised because the suggested point is not exactly “halfway the sharpest downward slope”. However I couldn’t tell if that’s good or bad until I train the model. For logging and visualization I used TensorBoard to log loss and accuracy during training and validation steps. Below you can see metrics history for each of four experiments.

![](https://enjoymachinelearning.com/assets/images/009_train_val_acc.png)

Learning rate suggested by Lightning (light blue) seems to outperform other values in both training and validation. At the end it reached 88.85% accuracy on validation set which is the highest score from all experiments (Figure 2). Also loss function values were the best for the “find_lr” experiment. In the last validation step it reached loss equal 0.3091 to which is the lowest value compared to other curves (Figure 3).

![](https://enjoymachinelearning.com/assets/images/009_train_val_loss.png)

### Conclusion

In this case, Learning Rate Finder has outperformed my choices of learning rate. Of course, I could have picked as my initial guess, but the whole point of LR Finder is to minimize your guesswork. Using Learning Rate Finder doesn’t require much additional code. In PyTorch Lightning you can enable that feature with just one flag.

I think using this feature is useful, as written by Leslie N. Smith in his publication:

> Whenever one is starting with a new architecture or dataset, a single LR range test provides both a good LR value and a good range. Then one should compare runs with a fixed LR versus CLR with this range. Whichever wins can be used with confidence for the rest of one’s experiments.

If you don’t want to perform hyperparameter search using different LR values, which can take ages, you have two options left: pick initial LR values at random (which may leave you with terribly bad performance and convergence) or use a learning rate finder included in your machine learning framework of choice.
