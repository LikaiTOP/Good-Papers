# Good-Papers

- Variational Inference with Normalizing Flows: https://arxiv.org/abs/1505.05770

One of the best papers I read recently! A very good note can be found here
https://casmls.github.io/general/2016/09/25/normalizing-flows.html

An open question, as described in the reference note: ``whether the proposed normalizing flows is a universal approximator of all the distributions when the number of layers go to infinity."
Two technical questions: Do we still need to assume P(z_0) as a Gaussian distribution? Why is the model not scalable regarding to high-dimensional latent space?

- MADE: Masked Autoencoder for Distribution Estimation: https://arxiv.org/abs/1502.03509

An inspiring work! It shows how autoencoder can be used for density estimation. Of course doing that is non-trivial as a conventional autoencoder can simply memorize all the data given large enough units in hidden layers, implying it cannot be used for the task. The paper addresses this problem by structuring the network so that its output represents conditional probabilities of a dimension given the others. They proposed masked autoencoder to do so.

Question: This is a very nice framework, but how to make it work with real value?

- Improving Variational Inference with Inverse Autoregressive Flow: https://arxiv.org/abs/1606.04934

An interesting paper. It combines two successful approaches in deep generative models recently: normalizing flow and autoregressive models. While the idea of normalizing flow is powerful, the authors claimed they are not scalable (but why?). Their intuition is that Gaussian autoregressive functions can scale well to high-dimensional latent space. The idea of using autogressive models seems a very good idea (e.g. see MADE, PixelCNN). The authors show that such functions can also turn into invertible non-linear transformations of the input. It therefore makes sense to use inverse Gaussian autoregressive functions as new flow. They show that their model achieve SOTA. A direct comparision between normalizing flow and inverse autoregressive flow is not performed, though!

It is quite surprising (at least to me) that applying many steps of conditional gaussian distributions can model arbitrary complex distribution. Why is that the case? I was wondering because in Normalizing Flows are designed to get rid of simple Gaussian distributions.

- Density estimation using Real NVP: https://arxiv.org/abs/1605.08803

A thought-provoking paper. Working with probabilistic generative models involve sampling (sample x given p(x)), inference (compute p(z|x) = p(z)p(x|z)/p(x) with latent (continuous) variables z) and exact likelihood computation (p(x) = int_{z}p(x|z)p(z)d(z). The paper proposes an interesting inversion models that address all the problems in a nice way. What I mean a nice way here is: exact sampling, easy to do inferenec and the likelihood can be computed easily. There seems no way of having such a model. But the author proves we can by exploiting bijective functions that map between each point of data (x) and a value of latent continous variable. The space of the variable must be easy to sample (e.g. a Gaussian: z ~ N(mean, variance)). Since we expoloiting bijective functions, it would be easy to find an inverse function of the bijective function so that given a point x, we can easily compute z (inference). Finally, the likelihood can be computed directly from the densitity function of z (it only involves additional computation regarding to the determinant of the Jocobian matrix with respect to the inverse function and x). Obviously, the bijective function must have a super rich representation to model arbitrary distribution, it must be easy and efficient to compute the Jacobian matrix (and the determinant of the matrix). The function they proposed relied on affine coupling layers. Relying on the idea everything can be done in a very neat way!

Question: how to train the model (maximize the likelhood of the data?) not sure I am missing something but they didn't describe that?
This is no doubt a very cool work, and I was wondering how to apply the model to NLP applications as well!
Finally, combining the model with VAE, GAN and autoagressive models could be a promising direction (see https://www.cs.toronto.edu/~duvenaud/courses/csc2541/slides/lec3-autoregressive.pdf)

- Wasserstein GAN: https://arxiv.org/abs/1701.07875 

It is hard to write a short summary for such a great work! Some good references are here http://www.alexirpan.com/2017/02/22/wasserstein-gan.html and here https://vincentherrmann.github.io/blog/wasserstein/ It has won widespread praise for its novelties and beautiful results. For me there are 4 take-home messages from the paper:
1. It shows that encoding a high-dimensional space into a lower-dimensional space is pretty much a task that traditional metrics such as KL-divergence and JS-divergence are not applicable (seeing the (abeit contrived) example to see why)). I am particularly interested in this, perhaps because I used to love KL-divergece very much (even after reading http://www.inference.vc/how-to-train-your-generative-models-why-generative-adversarial-networks-work-so-well-2/). Now we know for sure these divergences are not fitting to the problem, at least from the perspective of optimization.
2. It shows that Earth-Mover distance (a.k.a Wasserstein distance) should be a better match. Let us recall that probability distributions are defined by how much mass they put on each point. The distance between two distances P_original and P_new is the minimal effort we need to move the mass (according to P_original) for all points in the space to a new space with the new mass (according to P_new). I have a bit difficulty of understanding the distance in the beginning, but I was (as everybody) indeed very pleased with its the intuition. I don't re-explain it again, and a good explanation of the distance can be found here https://www.cph-ai-lab.com/wasserstein-gan-wgan) 
3. The Earth-Mover distance is very nice, but intractable to compute. Their ingenious approach is to rely on the Kantorovich-Rubinstein dualty to reformulate the computation of Earth-Mover distance (see this for an explanation about K-R duality https://vincentherrmann.github.io/blog/wasserstein/). Instead of computing the metric over each ``point" in the space, we can compute the metric based on the transformation of that point to another point in the same space. There is some constraint about the transformation, implying that the transformation function must be satisfied something. More particularly, they are 1-Lipschitz, i.e., functions that are differentiable and their maximum value of slope is 1. In this way, computing the distance amounts to finding a transformation function f that maximizes a new and different objective. They dualty doesn't change the difficulty of the problem, of course, but it makes our life easier to perform an approximation. Let us assume a parametrized function family f_w where w are the weights. A lower bound objective that can be close to the true distance is about finding the weights to maximize our new objective - a pretty much trivial thing to do with neural networks (i.e. backpropagation). Interestingly, the functions do not have to be 1-Lipschitz, as they could be K-Lipschitz (the paper explains very well why it is the case). But in principle, K should not be too large and too small. They clip weights w, constraining them to lie with a small range [-0.01, 0.01] to make it work (an adhoc and not a well-understanding solution, however). 
4. The training algorithm does not change much from the original GAN's training algorithm (it basically gets rid of ``unnecessary" logarithms and sigmoids). But we could gain pretty much from that (stable training). The new loss quite corresponds with image quality, which is pretty amazing. Too good to be true, right? Alas, that is not all their contributions. They even prove (Theorem 1 and 2) that everything comes for a reason!!!

What a paper! There is no doubt that this is one of the best papers I read recently!

- ImageNet Classification with Deep Convolutional Neural Networks https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks:


This paper ignites the trend of using Deep Learning and CNNs in Computer Vision, a must read for everybody I think. It does not have much novelty. But like Alphago, making old ideas (CNN) work well for such a very very very difficult task requires  expertise of Deep Learning (experience, technique) - the skills that those authors are second to none, if not the best. I had a bit confusing of the architecture, though. Specificially I was confused with the output of the first conv layer (55x55) when applied the layer to an input of size 224x224 w.r.t a filter of 11x11, with side 3 and pool size 2. The Stanford's course helps me clear http://cs231n.github.io/convolutional-networks/. Kudos to the lecturer, who is particularly well-known for his very best teaching skills. I also had a bit difficulty of understanding of the magic number of 4096 (the number of neurons in the fully-connected layer). You can find the link https://stackoverflow.com/questions/42733971/convolutional-layer-to-fully-connected-layer-in-cnn for an explanation.


Alexnet is a milestone in Computer Vision. Yet there are many ways we can improve. Let me mention several things:
1. Receptive fields applied in Alexnet are quite large in the begining (i.e. 11x11). It turns out that we should have a much smaller filter (e.g. 3x3). We should also stack multiple convolutional layers (instead of directly applying pooling layer after each convolutional layer). This leads to: (1) less parameters and (2) more nonlinearities in between. (see https://arxiv.org/abs/1409.1556 for a reference)
2. We can stack convolutional layers in a more complicated way instead of using a typical sequential manner. (GoogleLe net https://arxiv.org/pdf/1409.4842.pdf)
3. We can go around 100 layers with residual connections. This is a classic result from https://arxiv.org/abs/1512.03385. Amusingly, there is an interesting link between residual connections and boosting https://arxiv.org/abs/1706.04964


- Fully Convolutional Networks for Image Segmentation: https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf
It took me for a while to understand the paper, mainly because my lack of relevant background. The paper addresses one of the most difficult problems in AI: image segmentation (a.k.a. pixel-label labeling, dense prediction problem). Perhaps, its most important idea is to propose an end-to-end learning of a simple deconvolution to approach the problem. The deconvolution is implemented as bilinear interpolation, but it can be implemented as a deconvolution network that is composed of
deconvolution and unpooling layers (see this http://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Noh_Learning_Deconvolution_Network_ICCV_2015_paper.pdf)

There are several things I don't quite get yet, mainly with my very limited background:
First and most importantly, I wonder whether the final layers of the encoder have to be with the size of 1x1xdepth? I don't think it has to be that way (e.g. see https://arxiv.org/pdf/1511.00561.pdf), but I am not so sure why it is so common with the architecture (e.g. see http://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Noh_Learning_Deconvolution_Network_ICCV_2015_paper.pdf)
Second, I don't really have background of bilinear interpolation, and to me a decoder makes more sense than that technique. Why do the authors use it instead?
Third, I don't see why the encoder has to be a fully convolutional network. To me having several fully-connected layers at the end would be still fine (or could be even better abeit there are tons of more parameters).
Finally, applying Dilated Convolutions seems to be a very good idea to help image segmentation  (see this https://arxiv.org/abs/1511.07122). I admit I don't get this point though, but I believe this is just a matter of time and I need to think a bit more about it.



It is very pleased to know how people addressed this very hard problem with autoencoder-decoder. Nonetheless, I was wondering instead of using an encoder-decoder, are there other types of models that can produce such a heat map for each image?
