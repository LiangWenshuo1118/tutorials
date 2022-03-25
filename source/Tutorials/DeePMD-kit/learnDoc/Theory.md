# Theory

Before introducing the DP method, we define the coordinate matrix $\boldsymbol{\mathcal{R}} \in \mathbb{R}^{N \times 3}$ of a system containing <img src="https://latex.codecogs.com/png.image?\dpi{110}N"> atoms,

<img src="https://latex.codecogs.com/png.image?\dpi{110}\mathcal{R}=\left\{\boldsymbol{r}_{1}^{T},&space;\cdots,&space;\boldsymbol{r}_{i}^{T},&space;\cdots,&space;\boldsymbol{r}_{N}^{T}\right\}^{T},&space;\boldsymbol{r}_{i}=\left(x_{i},&space;y_{i},&space;z_{i}\right),(1)">

<img src="https://latex.codecogs.com/svg.image?\boldsymbol{r}_{i}"> contains 3 Cartesian coordinates of atom <img src="https://latex.codecogs.com/png.image?\dpi{110}i"> and <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{R}}"> can be transformed into local environment matrices <img src="https://latex.codecogs.com/png.image?\dpi{110}\left\{\boldsymbol{\mathcal{R}}^{i}\right\}_{i=1}^{N}">,

<img src="https://latex.codecogs.com/png.image?\dpi{110}\mathcal{R}=\left\{\boldsymbol{r}_{1}^{T},&space;\cdots,&space;\boldsymbol{r}_{i}^{T},&space;\cdots,&space;\boldsymbol{r}_{N}^{T}\right\}^{T},&space;\boldsymbol{r}_{i}=\left(x_{i},&space;y_{i},&space;z_{i}\right),(2)" >

where <img src="https://latex.codecogs.com/png.image?\dpi{110}j"> is the index of a generic neighbor of the atom <img src="https://latex.codecogs.com/png.image?\dpi{110}i">, and <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{r}_{j&space;i}&space;\equiv&space;\boldsymbol{r}_{j}-\boldsymbol{r}_{i}"> is defined as the relative coordinate.

In the DP method, the total energy <img src="http://chart.googleapis.com/chart?cht=tx&chl= E" style="border:none;"> of a system is constructed as a sum of atomic energies. 

<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;E=\sum_{i}&space;E_{i},(3)">

with <img src="https://latex.codecogs.com/png.image?\dpi{110}E_{i}"> being the local atomic energy of the atom <img src="https://latex.codecogs.com/png.image?\dpi{110}i">. <img src="https://latex.codecogs.com/png.image?\dpi{110}E_{i}"> depends on the local environment of the atom <img src="https://latex.codecogs.com/png.image?\dpi{110}i">:

<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;E=\sum_{i}&space;E_{i}=\sum_{i}&space;E\left(\mathcal{R}^{i}\right),(4)">

The mapping of <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{R}}^{i}"> to <img src="https://latex.codecogs.com/png.image?\dpi{110}E_{i}"> is constructed in two steps. First, each <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{R}}^{i}"> is mapped to a feature matrix, also called the descriptor, <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{D}}^{i}"> through an embedding network to preserve the translational, rotational, and permutational symmetries of the system. <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{R}}^{i}&space;\in&space;\mathbb{R}^{N_{i}&space;\times&space;3}"> is first transformed into generalized coordinate <img src="https://latex.codecogs.com/png.image?\dpi{110}\tilde{\boldsymbol{\mathcal{R}}}^{i}&space;\in&space;\mathbb{R}^{N_{i}&space;\times&space;4}">.

<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\left\{x_{j&space;i},&space;y_{j&space;i},&space;z_{j&space;i}\right\}&space;\mapsto\left\{s\left(r_{j&space;i}\right),&space;\hat{x}_{j&space;i},&space;\hat{y}_{j&space;i},&space;\hat{z}_{j&space;i}\right\},(5)">

where <img src="https://latex.codecogs.com/png.image?\dpi{110}\hat{x}_{j&space;i}=\frac{s\left(r_{j&space;i}\right)&space;x_{j&space;i}}{r_{j&space;i}}">, <img src="https://latex.codecogs.com/png.image?\dpi{110}\hat{y}_{j&space;i}=\frac{s\left(r_{j&space;i}\right)&space;y_{j&space;i}}{r_{j&space;i}}">, and <img src="https://latex.codecogs.com/png.image?\dpi{110}\hat{z}_{j&space;i}=\frac{s\left(r_{j&space;i}\right)&space;z_{j&space;i}}{r_{j&space;i}}">. <img src="https://latex.codecogs.com/png.image?\dpi{110}s\left(r_{j&space;i}\right)"> is a weighting function to reduce the weight of particles that are more distant from the atom <img src="https://latex.codecogs.com/png.image?\dpi{110}i">, defined as:

<img src="https://latex.codecogs.com/png.image?\dpi{110}s\left(r_{j&space;i}\right)=&space;\begin{cases}\frac{1}{r_{j&space;i}},&space;&&space;r_{j&space;i}<r_{c&space;s}&space;\\&space;\frac{1}{r_{j&space;i}}&space;\{&space;{(\frac{r_{j&space;i}&space;-&space;r_{c&space;s}}{&space;r_c&space;-&space;r_{c&space;s}})}^3&space;(-6&space;{(\frac{r_{j&space;i}&space;-&space;r_{c&space;s}}{&space;r_c&space;-&space;r_{c&space;s}})}^2&space;&plus;15&space;\frac{r_{j&space;i}&space;-&space;r_{c&space;s}}{&space;r_c&space;-&space;r_{c&space;s}}&space;-10)&space;&plus;1&space;\},&space;&&space;r_{c&space;s}<r_{j&space;i}<r_{c}&space;\\&space;0,&space;&&space;r_{j&space;i}>r_{c}\end{cases},(6)">

here <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{j&space;i}"> is the Euclidean distance between atoms <img src="https://latex.codecogs.com/png.image?\dpi{110}i"> and <img src="https://latex.codecogs.com/png.image?\dpi{110}j">, <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{c}"> is the cut-off radius, and <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{cs}"> is the smooth cutoff parameter. By introducing <img src="https://latex.codecogs.com/png.image?\dpi{110}s\left(r_{j&space;i}\right)"> the components in <img src="https://latex.codecogs.com/png.image?\dpi{110}\tilde{\boldsymbol{\mathcal{R}}}^{i}"> smoothly go to zero from <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{cs}"> to <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{c}">. Then a single value <img src="https://latex.codecogs.com/png.image?\dpi{110}s\left(r_{j&space;i}\right)"> is mapped to <img src="https://latex.codecogs.com/png.image?\dpi{110}M_{1}"> outputs, i.e. the first column of <img src="https://latex.codecogs.com/png.image?\dpi{110}\tilde{\boldsymbol{\mathcal{R}}}^{i}"> is mapped to a embedding matrix <img src="https://latex.codecogs.com/png.image?\dpi{110}\mathcal{G}^{i&space;2}&space;\in&space;\mathbb{R}^{N_{i}&space;\times&space;M_{1}}">, through an embedding neural network. 

By taking the first <img src="https://latex.codecogs.com/png.image?\dpi{110}M_{2}(<M_{1})"> columns of <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{G}}^{i&space;2}&space;\in&space;\mathbb{R}^{N_{i}&space;\times&space;M_{2}}">, we obtain another embedding matrix <img src="https://latex.codecogs.com/png.image?\dpi{110}\mathcal{G}^{i&space;2}&space;\in&space;\mathbb{R}^{N_{i}&space;\times&space;M_{2}}">. Finally, we define the feature matrix <img src="https://latex.codecogs.com/png.image?\dpi{110}\mathcal{G}^{i&space;2}&space;\in&space;\mathbb{R}^{M_{1}&space;\times&space;M_{2}}"> of atom <img src="http://chart.googleapis.com/chart?cht=tx&chl= i" style="border:none;">:

<img src="https://latex.codecogs.com/png.image?\dpi{110}\mathcal{D}^{i}=\left(\mathcal{G}^{i&space;1}\right)^{T}&space;\tilde{\mathcal{R}}^{i}\left(\tilde{\mathcal{R}}^{i}\right)^{T}&space;\mathcal{G}^{i&space;2},(7)">

In feature_matrix, translational and rotational symmetries are preserved by the matrix product of <img src="https://latex.codecogs.com/png.image?\dpi{110}\tilde{\mathcal{R}}^{i}\left(\tilde{\mathcal{R}}^{i}\right)^{T}">, and permutational symmetry is preserved by the matrix product of <img src="https://latex.codecogs.com/png.image?\dpi{110}\left(\mathcal{G}^{i}\right)^{T}&space;\tilde{\mathcal{R}}^{i}">.

Next, each <img src="https://latex.codecogs.com/png.image?\dpi{110}\boldsymbol{\mathcal{D}}^{i}"> is mapped to a local atomic energy <img src="https://latex.codecogs.com/png.image?\dpi{110}E_{i}"> through a fitting network. The fitting network is a feedforward neural network containing several hidden layers. The mapping from input data <img src="https://latex.codecogs.com/png.image?\dpi{110}d_{l}^{\mathrm{in}}"> of the previous layer to output data <img src="https://latex.codecogs.com/png.image?\dpi{110}d_{k}^{\mathrm{out}}"> of the next layeris composed of a linear and a non-linear transformation.

<img src="https://latex.codecogs.com/png.image?\dpi{110}d_{k}^{o&space;u&space;t}=\varphi\left(\sum_{k&space;l}&space;w_{k&space;l}&space;d_{l}^{i&space;n}&plus;&space;b_{k}\right),(8)">

In Eq.(8), <img src="https://latex.codecogs.com/png.image?\dpi{110}{w}_{k&space;l}"> is the connecting weight, <img src="https://latex.codecogs.com/png.image?\dpi{110}{b}_{k}"> the bias weight, and <img src="https://latex.codecogs.com/png.image?\dpi{110}\varphi"> is a non-linear activation function. It needs to be noted that only linear transformations are applied at the output nodes. The parameters contained in the embedding and fitting networks are obtained by minimizing the loss function <img src="https://latex.codecogs.com/png.image?\dpi{110}L">:

<img src="https://latex.codecogs.com/png.image?\dpi{110}L\left(p_{\epsilon},&space;p_{f},&space;p_{\xi}\right)=\frac{p_{\epsilon}}{N}&space;\Delta&space;\epsilon^{2}&plus;\frac{p_{f}}{3&space;N}&space;\sum_{i}\left|\Delta&space;\boldsymbol{F}_{i}\right|^{2}&plus;\frac{p_{\xi}}{9N}\|\Delta&space;\xi\|^{2},(9)">

where <img src="https://latex.codecogs.com/png.image?\dpi{110}\Delta&space;\epsilon">, <img src="https://latex.codecogs.com/png.image?\dpi{110}\Delta&space;\boldsymbol{F}_{i}">, and <img src="https://latex.codecogs.com/png.image?\dpi{110}\Delta&space;\xi"> denote root mean square error (RMSE) in energy, force, and virial, respectively. During the training process, the prefactors <img src="https://latex.codecogs.com/png.image?\dpi{110}p_{\epsilon}">, <img src="https://latex.codecogs.com/png.image?\dpi{110}p_{f}">, and <img src="https://latex.codecogs.com/png.image?\dpi{110}p_{\xi}"> are determined by

<img src="https://latex.codecogs.com/png.image?\dpi{110}p(t)=p^{\operatorname{limit}}\left[1-\frac{r_{l}(t)}{r_{l}^{0}}\right]&plus;p^{\operatorname{start}}\left[\frac{r_{l}(t)}{r_{l}^{0}}\right],(10)">

where <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{l}(t)"> and <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{l}^{0}"> are the learning rate at training step <img src="https://latex.codecogs.com/png.image?\dpi{110}t"> and training step 0. <img src="https://latex.codecogs.com/png.image?\dpi{110}r_{l}(t)"> is defined as

<img src="https://latex.codecogs.com/png.image?\dpi{110}r_{l}(t)=r_{l}^{0}&space;\times&space;d_{r}^{t&space;/&space;d_{s}},(11)">

where <img src="https://latex.codecogs.com/png.image?\dpi{110}d_{r}"> and <img src="https://latex.codecogs.com/png.image?\dpi{110}d_{s}"> are the decay rate and decay steps, respectively. The decay rate <img src="https://latex.codecogs.com/png.image?\dpi{110}d_{r}"> is required to be less than 1. The reader is referred to the original papers of DeepPot-SE (DP) method for details.


