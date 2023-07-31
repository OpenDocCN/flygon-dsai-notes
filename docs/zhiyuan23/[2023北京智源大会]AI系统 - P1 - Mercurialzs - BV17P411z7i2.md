# [2023北京智源大会]AI系统 - P1 - Mercurialzs - BV17P411z7i2

[音乐]，[音乐]，OK 谢谢大家，[掌声]，OK good morning，Since we have some invited very important speaker here。

and we have some foreigners here，so allow me to give this opening，in both the Chinese and English。

Because we don't have the automatic translation system，for this workshop，so let me do it by myself。

大模型时代，当我知道要chair这个workshop的时候，我一直在想是说，What's the important thing for the，for the large model。

and especially，what is the important topic for the AI system，in this new very important new trend。

for the to support the，the model to go larger and larger scale，大模型时代到底，AI system，在组织这样的workshop。

我们应该放什么样的topic进来，能够让AI system，更好的支撑大模型的发展，那 of course I think。

the very important thing is the performance，is the large scale system performance。

to support the training，especially the training，最重要的其实是这个系统怎么样子，可以去很高性能的，去支撑这个训练系统。

and the other thing I think is，the what kinds of chipset，including the，[音量]。

the chipset and including the，the other architecture innovation，to support the AI system，另外一方面是底层我们有。

什么样子的AI的chipset，包括英伟达一代一代的新的chipset，也包括我们很多新的，芯片的架构，and so that's why we have。

we arrange today's workshop，including the high performance parallel optimization，for deep learning。

and the different kinds of chipset architecture，so 所以今天我们很高兴，邀请到了几位专家来，代表着我们的，包括PyTorch 包括Ray。

也包括NVIDIA Mectrum这些的，高性能的去优化我们的，尤其是训练的这个架构的性能，同时也邀请到了几位代表，芯片厂商的tech leaders，来介绍在这方面他们的insight。

but in between，the algorithm and the chipset，very very important is the compiler，AI compiler，但是大家要注意。

其实对我们越来越复杂的大模型的算法，和下面越来越设计的很高性能的，我们的硬件的GPU和Oscillator的架构，很重要的是中间的AI的编译器。

so today I also very glad that，we can invite a very typical，very famous compiler in AI。

including the PyTorch AI compiler，and including the MLIR，and also the other。

maybe the next generation AI compiler，and also some academic research。

for how to automatically optimize，the things for the compiler，so today I also very glad that。

in such an important field of AI compiler，we also invited a very typical。

including the PyTorch AI compiler，including the MLIR，from Google。

and also maybe the next generation AI compiler，including the academic research。

for how to automatically optimize，the things for the compiler。

okay so today I don't want to spend any longer time，I just want to invite the speaker。

because we have a very high schedule，and because so many things for the AI system today。

所以我们就不再花其他时间了，okay so let me introduce the first speaker，Albert Cohen。

yeah let me have an introduction for him，Albert is a research scientist，at Google。

and he graduated from the University of Breslau，sorry I don't know if I pronounce it correctly。

no matter，and actually he is I think more thing，he is now work for Google。

and a lot of thing related to the MLIR，and but he is the very active guide。

did a lot of research and work for the GCC before，yeah actually he is the very famous person。

in the compiler area，so let's welcome，thank you very much，谢谢。

thank you for the very kind introduction and invitation，I'm very excited to be here。

so I try to speak not too fast，but I have an attractive tendency to speak very fast。

so we are talking about large scale models，language models in particular。

but I will talk about small things，like mostly by making the most。

I mean more like in the bottom up fashion，so if you want to get the best possible performance。

at large scale，you have to start small from like the instruction sets。

low level vectorization and caches，and optimization for the，that are very hardware specific。

so that's what I'm talking about mostly today，about，and essentially my background。

as you already mentioned Yonghe a bit，I've been working in compilers，for more than 20 years。

most likely you are aware of some of，the work in the parallelizing，and so called polyhedral compiler。

context if you have worked in the area，but I don't want to talk much about this today。

we are not into looking for parallel loops，into legacy code anymore，we have the chance that。

because of machine learning workloads，parallelism is all over the place。

the question is how do you make good use of it，how do you exploit all these。

all these degrees of freedom，all these domain specific languages for ML。

and to extract the information you have，put it into good use。

for reaching the best possible performance，and in this space，I've been working both on。

applying machine learning to compilers，like 15 years ago，and then I had a break，work on other things。

and then came back to using that，again ML in compilers，more like new type of ML，like deep learning。

and also building compilers for ML，more recently，originally at INRIA and Facebook。

and then I joined Google，more than 4 years ago，so that's a quick introduction。

so why ML and compilers，so you already said it，but if you want to get，the best possible performance。

out of any like domain，you need some kind of compiler，that will assist。

the translation of the abstraction，of that domain to get，the best possible performance。

of the hardware，so let me give some concrete，extremes or examples，some people care mostly about。

some people mostly care，about peak performance，like most of the people，working on language。

[inaudible]，have to rely on some form of，operators optimized，[inaudible]。

optimized for a given piece of hardware，and other people，mostly care about，performance portability。

meaning that you want，for example a model，developed on a data center，to be easily portable。

to a mobile device，and I took some kind of extremes here，like the wafer scale processor。

from Cerebrus，and the edge TPU from Google，there are many of course，equivalent chips。

for mobile or edge devices，so how do you get both，typically how do you get。

the peak performance you can，for a given chip，and also how do you make it easier，for ML programmers。

to port their models，across all these different scales，so that's what。

compilers are going to help you do，and so this is tough，because of this hardware diversity。

all these different scales，but also on the software side，things are not very nice。

although it's improving a little bit，so there are many ML frameworks，of course TensorFlow，JAX。

 PyTorch，many other frameworks，and also the model size，and complexity is really。

increasing very quickly，so maybe some of you attended，the presentation of my colleague。

Yanqi yesterday，on the mixture of experts，so the complexity of the models，is growing with。

typically the size of the data sets，the multi modalities，and all the optimizations。

you may want to implement，make the models more and more，control intensive。

so it's not only matrix product，but at the bottom，you still have like matrix product，anyway。

but the complexity is increasing，and that's really big difficulty，for ML compilers as well。

so let's take a step back，and try to understand，what ML compilation，really means in practice。

so the state of the art is，as any compiler would look like，so you have a high level language。

in this case it's a domain specific language，of standard operations，for example a Python。

embedded domain specific language，like PyTorch or JAX，and you have some methods，to analyze graphs。

of some of these operations，apply high level transformations，like automatic differentiation。

different ways of parallelizing，etc。，and then there is some kind of，magic thing underneath。

that's called a tensor compiler，that will translate these graphs，into efficient code that runs on。

CPUs， GPUs， TPUs，all kinds of hardware，so that's a very rosy，very beautiful picture。

but in practice it's not，quite really that nice，it's not that glorious，as I prefer to say。

on some hardware compilers，are really doing a big part of the work，like on Google TPUs。

there is no real，like independent library，basically nobody can program it。

outside using the XLA compiler，for TPUs，on GPUs from NVIDIA，it's quite different。

so you have lots of NVIDIA engineers，writing library code like CUDNN，that is actually doing。

most of the effort，if you're running，like transformer models，most of the time is spent。

on CUDNN operators，and heavily optimized，attention layer for example，same thing for CPUs。

and most hardware companies，are spending quite some time，writing hand optimized libraries。

so there is some level of automation，usually but it's eventually，very hand optimized code。

so what's the role of the compiler，in this space，if you're using libraries eventually。

so that's a real question，and I'm kind of anticipating，the next talk。

I'm sure that Peng Wu will tell you，that compilers are great，but typically most ML programmers。

will see it as a bonus，so if you can compile，and get better performance。

with little effort it's great，I'm not going to waste your talk，(laughs)，and so the。

it's why is it a problem，actually there are many reasons，one reason is that。

it's difficult to implement fast，neural network layers by hand，of course as you can imagine。

it's most practitioners，don't know how to do that，you need to be a high performance，computing expert。

you need to know the platform very well，and the benefits of doing that，can be huge。

so if you write naive code，it can be 100 times smaller，than the sorry slower，100 times slower than。

like peak performance，and so basically，anytime you do research，on a new model，or trying to tweak。

an attention layer a bit，or convolution layers a bit，doing some kind of custom fusion。

you have to redo that，you have to implement again，the low level，operators on your machine。

and that's very inefficient，so maybe some of you，heard about the Triton。

project and language from OpenAI，TVM also is a library in that space，and the language in that space。

that's very successful，so there is a lot of need，for writing this kind of，custom operations。

and optimizing them，but it's still，it's still very tedious，and you would rather not。

go back to writing low level code，if you had a good compiler for it，so what about，ML compilers today。

based on this picture，there was this ML compiler，are still there for a reason，and the main task。

that ML compilers，are very successful，at dealing with today，is applying high level transformations。

so like automatic differentiation，I mentioned，so I'm mostly looking into，jax in this example。

but also high level，distributed computation，automation vectorization，and of course。

figuring out at which level，you want to apply，just in time compilation，so if you are able to。

I'm actually hiding the slide，I remember，if you can actually control，at what function granularity。

you want to apply，a JIT compiler，like XLA in the case of Jax，you would typically do that。

with this type of low level，of sorry，very high level abstractions，so that's great benefit。

because you don't have to think，as a ML programmer，in terms of low level optimizations。

everything is happening，behind the scenes，in these high level transforms，like auto diff。

and in the JIT compiler，the problem is that，this is still very high level，you're only doing。

like tracing the computation，building syntax trees，optimizing like building graphs，automatically。

recognizing graphs automatically，and maybe iteratively trace，and transform those graphs。

until you get to some，cluster of operations，that you can optimize together，and most of the magic。

is still happening down there，if you want good performance，you need something to actually。

deliver good performance on the target，and this is not this，big cycle of tracing，and transformation。

that will do it，you need a library，or you need a compiler underneath。

and that's what I'm focusing on here，so the type of benefits，you can get from compilation。

there are typically，there are numerous，but the main ones are，getting rid of，interpretation overhead。

that's the most likely purpose，of using a compiler，that you don't want to，dynamically interpret。

and dispatch the logic，for different operations，you can also use this，for fusing operations。

the memory hierarchy，like the caches，local memories，have to be leveraged in a way。

and that's typically through fusion，that you can do that，so avoiding round trips，to main memory。

or across device，and host interfaces，you can also do a lot of，optimizations at that level。

by loop tiling，unrolling vectorization，et cetera，that will be necessary。

to leverage specific hardware，and it's also very important，for reducing the executable size。

the code size，especially on embedded，or mobile devices，you can specialize the code。

to one very specific model，unlike libraries，which have to provide operations。

for every possible model basically，so it's also used，in embedded space for that。

and also a very important thing，you can do auto tuning，so with libraries，you have to think ahead。

of all the possible shapes，all the possible operations，context like layout et cetera。

when you are compiling，you can specialize，on one particular shape and layout，and the context。

in which an operation executes，so that also gives rise，to interesting，automatic tuning techniques。

but OK，I have to be very humble here，because I've been working，in the area for 20 years。

advertising for these great benefits，but we are still there，I mean are we making，any progress here。

and basically the ML now，is everything，that's what paying the bill，but the problem is still there。

although we live in，a more specialized world，so we have to think twice。

and I want to briefly advertise，this auto tuning effort，from some colleagues。

presented at the PAC conference，a couple of years ago，if you want to learn more，about auto tuning。

because this is a big part of our job，this is one great example，of things happening。

in the space of XLA for TPUs，but we are also doing that，for other targets，just advertising。

for this piece of work，led by my colleague Meng Po，and more generally，if we want to make progress。

actually I'm coming here，not to tell you how to do it，I'm mostly coming here。

to ask for help and collaboration，because after 20 years，we are still there。

and so I'm going to ask a few questions，like through some proposals，in the rest of the presentation。

so my first claim is，I think we need to move，from compiling for ML，into more and more ML。

using compilation，so of course this is not new，I worked on it like 15 years ago。

there was work before that，but it's still very much immature，so there are not so many。

production compilers，that actually use ML techniques，a little bit of auto tuning offline。

but not so much，when you are actually，actively compiling code，the main reason is that。

it has been of course，a difficult problem to optimize，like NP complete problems，for like scheduling。

register allocation，et cetera forever，but now we are dealing with。

very hardware specific optimizations，that are even hard to model correctly。

so we don't have a cost model，we don't have an analytical means，to reason about performance。

typically the only hope is that，the machine itself can learn，what is good，what is profitable。

what has to be optimized，for a given operation，or graph of operations，and one approach to do that is。

I would call it controllability，so you want the compilers，to be more open，in the sense that。

they are more gray boxes，so you want to intervene，as an expert performance engineer。

into how the compiler generates code，so it's the exact opposite。

of what I was showing at the beginning，where you have a black box，very abstract。

you say JIT and it's magical，in this case it's not magical at all，you want to intervene and say。

I know how to optimize，this particular operation，please do this for me automatically。

because otherwise I have to write，Triton code or CUDA code or something，so scheduling languages。

that have been popular for a reason，but on top of that，you still would like to automate。

those schedules，so you want to infer basically，you want to search basically。

what's the best possible schedule，for a given machine，and given shape， etc。，and you want to also。

automatically build the schedules，for you using machine learning techniques，so that's I think。

one very important thing，and schedules，there are actually，many different types of schedules。

or scheduling languages，in high performance computing，actually it even started，like even earlier。

like in the early 90s，there were some ideas in that space，in polyhedral compilers。

I worked on some of these techniques，in like 15 years ago，with a language，that we designed also with。

Nicola Vasilake at Facebook，we had this tensor comprehension work，that also had schedules。

and now Halide and probably TVM，you know are very popular in that space，basically Halide changed the。

changed the rules of the game completely，making these scheduling languages，much nicer to use。

and much more embedded，into domain languages，so it's not new。

there are lots of work at NVIDIA as well，etcetera in that space，but and oh yeah。

yeah that's the one I was referring to，30 years back we still，we already have this type of test。

but I'm not expecting you to read that，so why schedules，the main reason people want schedules。

as I said is，performance engineers want to be able to，control what the compilers do。

because if they cannot，they will never get the peak performance，so if you want some kind of way。

to reuse the existing logic in the compiler，without having to completely，forego the compiler。

and write low-level CUDA code，on a GPU，and one typical example is，when you have a clear optimization。

methodology in mind，so in this case it's focusing，on matrix product and convolutions。

is the BLISS methodology，that is essentially sketching，how matrix product。

matrix multiplications are implemented，on any accelerator，or even on CPUs these days。

and you have to decompose，your computation into layers of tiling，and at the bottom。

you have like some very optimized，vectorized unrolled code，that will be very specific。

to your machine，and actually I figured it，in the like top-down way。

but the real way these methodologies，are implemented is more like bottom-up。

so you first write micro kernels，of heavily optimized unrolled loops。

and vectorized and vector instructions，and then you slowly grow，like more like unclosing loops。

around these micro kernels，to reflect the memory hierarchy，and the parallelism hierarchy。

of the machine，and so this is this BLISS methodology，that is quite popular。

we did some work in the area as well，and realizing that in fact，we can build search spaces。

we can build scheduling languages，that have these kind of methodologies。

captured in a more specific way，into the scheduling language，and into the search space。

so that was a paper that，I mean the tool was called T-Tile，but it's not in the title。

that was joint work with INRIA，and colleagues from University of Utah，in the US。

and we essentially showed that，there are ways to automate，the process a bit more。

than using TVM for example，and I'm not going into details，but the main intuition here is that。

you can actually have，more than one micro kernel，if you look at the，like 1DNN library of Intel。

the MKL library of Intel on x86，you essentially have three micro kernels。

written in assembly language，if you look at CUDNN or KUBLAS，on NVIDIA it's about 20，but you can have。

more than one micro kernel，that have to be generated automatically，if you want to。

some level of portability，or in the worst case，you write them by hand，but you'd rather not do that。

but you select the best micro kernels，in this case，you have this kind of。

Crescent shape of micro kernels，depending on the unrolling degree。

on the size basically of the micro kernel，and on top of that，you can grow the rest of the stack。

completely automatically，so I don't have time to go into details，but let me skip that。

the scheduling language，can be extremely simple in that case，so you don't have to write。

very complex sequences，of transformations，you can only say，I have like five basic primitives。

for organizing the computation，above the micro kernel，I can like decide on，loop unrolling factors。

I can decide on what to vectorize，like which dimension，is it like row， column， etc。，vectorization。

I have to figure out，tile sizes like block sizes，if you're on a GPU，how many threads，how many blocks。

 etc。，you're going to have or so，and the rest is，so you can，yeah， you have to generate。

the rest of the control flow，this is the this R construct，and you have this。

fancy lambda thing at the bottom，that is a way of sequencing kernels，or micro kernels。

so you need to accommodate，for one very painful thing，in hyperfunce computing，which is that。

all the data，all the sizes，all the shapes，are not powers of two，in practice。

I mean actually in transformers，more and more，it's like powers of two，but on convolutions。

it's definitely not powers of two，you have very fancy thing，like 34 or 17，or something like that。

not sometimes prime numbers even，so you have to be able to，compose multiple micro kernels。

until you reach the proper size，so I'm sorry I'm going fast，I'm not explaining those things。

in detail but，but it's a very，it makes the search space，much easier to traverse，at much smaller。

when you have such a，very specialized scheduling language，and in this case，in this graph。

we are comparing the TVM，autoscheduler called ANSO，which is the blue bar，so blue graph。

so TVM works by，chunks of 64 runs，like empirical evaluations，on the machine，and every 64 runs。

sorry this is log scale，at the bottom，but this is 64，it will try to select，using a cost model。

that is being learned，the best possible scenario，best possible optimization，using TVM schedules。

and if you use our schedules，which are more specialized，and compressed，you can very。

you can very quickly converge，like maybe 10 times less，or even less than that。

so just a demonstration，of working on the scheduling，language helps，if you want to reach。

the automatic，automatically the optimal solution，so I'd like to make，one proposal here。

and this is really again，calling for help，ok we see that，there is some room for。

automation of schedules，and writing more，domain specific schedules，but there is still one problem。

that we are facing，it's going beyond，those static shape，like specializations，so when you know。

everything about the layout，the shape is great，you can do this，completely automatic search，offline。

and you get the best，possible implementation，for one very specific operation，but in practice。

you still like to bridge the gap，with what libraries offer，like QDNN， QBLAST， etc。。

they provide you with，a one-stop shop basically，for all possible shapes，and it's kind of frustrating。

that you have these libraries，that are very generic on one side，and these compilers。

that do automatic tuning，for just one possible shape，so I call it TLO，Tile Level Operations。

so Google doesn't have，anything there，we're just asking questions，but we'd like to。

devise some kind of interface，where instead of thinking，of a high-level library of operations。

that are essentially the ones，you would write in NumPy，or in JAX or whatever，ML framework。

you have a library of operations，that is automatically synthesized，for a given model。

and given hardware，at the tile level，so I would argue that，the compiler is extremely useful。

as this Blizz methodology shows，to generate optimal code，at the tile level。

like maybe on a block on the GPU，or any code that runs on an L1，or L2 cache size on the CPU。

but you don't really need the compiler，to orchestrate all the computation。

at the level of the full node，it's quite intuitive，that you would have，some kind of runtime system。

some kind of dispatch logic，at some point，that is going to be interpreted。

if you're close to the instructions，you need to compile，otherwise it's too expensive。

but if you are at the level，of like full operations，you don't really need a compiler。

the dynamic dispatch can be fast enough，so there is this kind of boundary。

between tile level operations，that are compiled underneath，and above that boundary。

you have interpretation，that can quickly dispatch，across these different operations。

the challenge here is that，you don't want those operations，to be decided by an expert。

you want those operations，tile level operations，to be automatically synthesized，for a given domain。

and I call it，like a 2D parietal surface，so you want to trade，basically performance，for code size。

for specialization，on a given domain，so if you know exactly，you're only going to run。

like a large language models，with these particular shapes，it's no need for optimizing，for like LSTMs。

you're only optimized，for that particular shape，and model architecture，but still I think the problem。

is reasonably well defined，there is no solution to do that yet，I mean a couple of papers。

are coming close，but if you're interested，I can mention them offline，so briefly about MLIR。

since you mentioned MLIR earlier，so we are building infrastructure，to address these issues。

like schedules，autoscheduling，compiler construction，more portable reusable etc。

using the MLIR framework，so I'm not going to give you，a course on MLIR，we don't have time。

and there are plenty of resources online，and many of you，already know about it，but it's。

MLIR has been designed，for extensibility，so basically it doesn't do anything。

but it's great framework，for implementing compiler flows，and also bridging frameworks，for example。

you may be aware of something，called stable HLO，which is now being used，by many frameworks。

including PyTorch，to communicate with XLA，and there is this recent。

open XLA infrastructure initiative，that is endorsed by many companies。

which is not governed by Google anymore，so it is a separate organization，of course Google was there。

to start it up，but it's a community really，like LLVM like MLIR，to build this next generation。

portable compilers for ML，and so I'm skipping MLIR examples，sorry just mostly to focus on。

one aspect of MLIR，which is extensibility，but in extensibility，in a very specific way。

about how do we build schedules，in an extensible way，I told you schedules。

are very important for performance，very important research area as well。

but we don't want to start from scratch，we don't want to implement，a new scheduling infrastructure。

like TVM from scratch，and using MLIR，what we can do is that，we can start from a baseline。

optimization flow，that takes the intermediate，representation of the graphs，that you are optimizing。

some like hand-coded optimization，and heuristics and generate code，and you can extend it。

with the same syntax，and the same logic，and abstractions of MLIR，with another IR。

which represents the transformations，this time，so for example，if you want to teach the compiler。

to do fusion of matrix product，and softmax，for attention layers，you can explain those。

the logic of this fusion，in terms of rewriting rules，in terms of patterns，in the IR itself。

and you can drive this，transform IR，with either by an expert，writing a schedule by hand。

so it's very painful，because it's an MLIR logic，but on syntax，but you can of course。

use a DSL on top of it，but so you can write this IR directly，saying I want specifically。

to fuse this matrix product，with this softmax，or you can also embed，a database of transformations。

that have，that can be searched automatically，so you can add，so some AI around it。

some auto tuning logic，that will automatically，schedule this transformation。

so you can make everything，every component，that was showing earlier，with schedules。

and automatic scheduling，as an extensible，component of MLIR，and so this is something。

we are working on，so there are also some extra logic，maybe a little bit of C++，you have to provide。

for some transformations，but most of the time，you don't have to do it，and so if you're interested。

in this approach，so MLIR can help you do that，like building your own，scheduling languages。

your own custom TVM，if you want，we call it the transform dialect，and there was a great tutorial。

given by my colleague Alex，Zinenko was the main，actually contributor of that project，at the MLIR。

at the LLVM sorry，deaf meeting last month，so you can look it up，and there is also a paper。

that was published recently，so I don't have time to，go into more detail。

I can conclude by just saying again，we have problems to solve，some of them we know well。

some of them we have no clue，so we are very interested，in collaboration，if you are in this room。

you already know that，so that ML is not just about，data models and compute。

but it's also about compilers，otherwise you will not be in this room。

and it's also just the beginning，so we have been working for 20 years，and actually many more years。

on this type of，high-performance computing techniques，but now in the field of ML。

we have much more advanced，ML techniques to use as well，so we should really bring those。

into the compilers as well，thanks for your time，(掌声)，OK， thank you，sorry，yeah。

so because the very tight schedule，so I didn't arrange any Q&A，for this，for today's any speaker。

but welcome for any of your questions，of line， yeah，so next I want to introduce Peng Wu，yeah。

actually the one special thing，I want to mention，and I don't know how many of you。

have watched the launch of，PyTorch 2。0 that event，actually I online to watch that。

and right after the PyTorch 2。0，announced，and the first invited speaker。

as the some important technical announcement，the first one is Peng Wu。

and she announced the compiler Dynamo，for PyTorch 2。0，and at that time I think，wow。

 PyTorch have the compiler now，so that's be something special，or beginning things for PyTorch。

so Peng actually she have，work for IBM research，more than 10 years。

actually I also work for IBM research，for the system for more than，18 years as well。

and she later work for Huawei，and established the，I remember the programming tech lab，yeah。

and work for Huawei for 7 years，and then she joined the Meta，and let the compiler。

part for Meta for 3 years，okay let's welcome Peng，yeah，okay thanks for the introduction。

you might notice on this agenda，we have two PyTorch 2。0 talks，I didn't realize there is a second one。

but I want to qualify this one，so this one is focusing on，how we are bringing compiler。

to the core of PyTorch，and then later the next talk，Michael is gonna focus more on how，PyTorch 2。

0 is helping，the large language model training，so you've already heard one compiler talk。

and I saw in the agenda there are more，so before I start，I want to give you a mental model。

of like not all compiler，ML compilers are the same，so one basic way to differentiate is。

in my mental model is to differentiate，between machine learning framework compiler。

and machine learning accelerator，or hardware compiler。

so our best talk is more coming from the bottom，from like closer to the hardware。

and because our compiler，is called PyTorch compiler，so we are more coming from the top。

and the thinking is very different，because as a framework compiler，what we are focusing on is。

how can we enable all these hardware vendors，to bring PyTorch models to their hardware。

so there are things we do，we have to do，and there are things that we want the vendors to do。

so keep that question in mind，on the thinking behind，a machine learning framework compiler。

so today I'm gonna tell a story，I know there are many people，are building their own chips。

or even their own machine learning frameworks，I hope at the end of the talk。

you could get a glimpse of the thinking，of how you're bringing compiler to PyTorch 2。0。

not for the interest of research ideas，really for making real models run faster，All right。

 so this is the release note，early this year， March 15th， 2023，this is where we actually release。

the PyTorch 2。0 binary，and it says that，we want to offer the same。

eager mode development and user experience，while fundamentally supercharge。

how PyTorch operates at the compiler level，under the hood，so let me just simplify the message。

I want you to remember two words，graph mode and ease of use，so it was long believed by the industry。

that you can either have graph mode or ease of use，but you cannot have both，and in this PyTorch 2。

0 story，I'm gonna challenge that conventional wisdom，so essentially I'm gonna show you。

how do we have the cake and eat it too，but before I get to 2。0，let's roll back five years to 1。0。

so PyTorch 1。0 was announced，almost exactly five years ago， 2018，at the time。

 almost all the major industry-backed，machine learning frameworks are choosing graph mode。

because it was believed， it's rightly believed，so that with graph mode。

you would have the ability to optimize using compiler。

and the performance ceiling would be a lot higher，if you optimize one op at a time。

so PyTorch was actually a late comer，in terms of machine learning frameworks，and at the time。

 we made a critical，also risk of decision to value ease of use，above everything else。

the thinking is that we want to cater to，machine learning researchers，and for researchers。

 they actually value，more time to market than performance initially，but with that work。

 we actually don't know，so what we did is actually，we are one of the few probably。

just go all in with non-graph mode，and then we compensate on the performance side。

by working very closely with vendors，to optimize at the library level，and optimize whatever we could。

within the constraints of eager mode execution，so the data showed that this bet paid off。

so since the 1。0 announcement，at the time， we are about 30% of researcher adoption。

and as you can see， the curve is going up，and around two and half years mark，after the 1。0 release。

we have passed the 50% mark，and that essentially meant that。

we are the number one machine learning framework，used by machine learning researchers。

and you see a little bit of dip over there，so actually I double clicked into that。

so this is based on same data source，but rendered in a different way，so the middle section。

 the red part，is a PyTorch kind of researcher market share，and when you see the dip here。

is actually more about a few，new up and coming machine learning frameworks。

and because this is in China，so I want to highlight a few，homegrown machine learning frameworks。

so there is the Mindspore， I believe here，which sees a significant increase。

actually I used to work on the Mindspore，the accelerator compiler，and then there is also the Petal。

 the purple one，let's see， and JAX is this green one，and then there are like a lot of other things。

and from this picture you would see that，there are a lot of players over there。

but PyTorch because of this ease of use，is really becoming the anchor of。

a lot of the kind of the framework of choice，for researchers，all right， so now let's go to PyTorch 2。

0，what this talk would be focusing on，one specific feature，but a fundamental feature of 2。0。

we introduce this very simple API，Torch。compile as the primary API，for introducing graph mode。

and what this does is，remember ease of use and graph mode，so basically what you are right。

when you are writing the model，you are still thinking in terms of eager。

so this is very different from，the traditional graph mode based ML framework。

in those framework you have to think about graph，and that's why researchers didn't like that。

it's just not intuitive，very hard to debug，so here you write as if you are in eager。

but you do an annotation，just one line change，to indicate to the compiler。

this is a part that we want to，make it work under graph，so it's very very simple， right？。

so does it work？，and actually in the PyTorch conference announcement。

last December this is the data we showed，and we're showing over 170 models，Tim is a vision model。

Torchbench is a bunch of，highly popular research models，and Hugging Phase is more like the。

today the transformer based models，and the results are run on a media A100，and we combine results。

between AMP and Flow32，with more weights on AMP，because that's more performing for training。

and all these data are for training，so these are the，geo mean performance speed ups。

that we're reporting at the time，and today the number has further increased，alright so。

you might wonder，why do I make such a fuss about graph mode。

almost all other machine learning frameworks，use graph mode， right？，so the problem is that。

because of PyTorch design for flexibility，and expressiveness，those good traits actually made it。

very hard to compile，or to introduce graph mode with，so if we look back on 1。0 and 2。0。

I could say that 1。0 is about a strategic decision，of like who are we catering for。

and what are we giving up，and what are we embracing for，so that's 1。0，for 2。0 it's really about。

actually the emergence of 2。0，was really based on technical innovation，so the moment we figured out。

how we can get the graph mode，without sacrificing ease of use，that's the moment that 2。

0 becomes real，and that moment is a Torch Dynamo moment，Torch Dynamo is an out of the box。

graph capture for PyTorch，so if you have followed about PyTorch compiler。

I mean people didn't know about PyTorch compilers，maybe two， three years ago。

because we have too many of those，so we have tried many times，and our previous graph capture。

always require significant manual effort，when you are trying to work with real models。

so this manual effort ranges from，either you have to change your graph or model。

to make it capturable，or you have to，or if you are able to capture a graph。

you have to make sure it is correct，when you replay it，so Torch scripting， FX， Torch tracing。

Lazy tensor are all the previous generation，graph capture mechanisms，so how do we solve that problem。

to make the graph capture reliably，capturing the correct graph。

basically how do we make the mechanism，both sound and out of box，so let me give you some intuitions。

so the first insight we have，is that all the previous graph capture。

is aiming at capturing one graph model，and that's where all the constraints，are coming into play。

where if there is something，that the compiler didn't understand。

because Python is a very flexible language，so what we decided is to，allow capturing partial graphs。

so that whenever we encounter，something that we didn't like，we would just stop the graph break。

and fall back to eager，and for soundness，because part of the graph capture。

is using tracing mechanisms，so we introduce guards into the graphs，and this would allow us to record。

what's the valid condition，for this graph to be correct，and in order to use these guards。

at runtime we need to check，whether the runtime condition，satisfies the guard condition。

and if it doesn't，then we'll just in time recapture，so it operates almost like。

a just in time compiler，so to give you one example，for the previous example。

this one I deliberately introduced，one which requires a graph break，so if you print out the graphs。

to the right hand side，of what we captured for this example，you without if you didn't know about。

all the previous points，you might be surprised to see，actually three graphs，that are highlighted by。

the colored bar over there，because there is a data，dependent control flow，b。

sum depending on the value of b，it may or may not go into，the if or else branch。

and but there are other reasons，that we graph break，so for example if we have something。

that's like outside the Python，or C extension，we don't know anything about it，if we will graph break。

for soundness reasons，and there are some of these conditions，actually where over the last。

couple of months，we're gradually reducing，the reasons for graph breaks。

some are implementation limitations，some are just fundamental，to the correctness of the program。

I talked about graph breaks，but I also want to show，like we are supporting。

this in torch dynamo graph capture，we're really supporting，a lot of very complex features of Python。

so here is a list of the complex things，how do we handle function call。

how do we handle comprehension，and all these container types，loops control flow lambda。

and so on and so forth，so I want to give you a glimpse，of what the magic under the hood。

so this picture，the left hand side is the default，c Python behavior，so this is what happens。

when you use eager mode，and to the right hand side，especially the dotted box here，and there。

those are the new things we added，number one，it's actually very very lucky，we have a standard Python。

c Python feature，that allows us to add these extensions，so that means you don't have to。

download a special version of c Python，you just need to use，a particular modern version of it。

and here with this box is，the thing we talked about，capturing partial graphs，break and then。

compile the graph into binary，and then execute the compiled code，doing the one time check。

going to a cache，and recapture，all of these things，happen transparently，behind the scenes。

so that's why users，just have to annotate，one line of code，in order to go into the graph mode。

so these things seem to be plausible，does it really work，we actually run torch dynamo。

with a very simple backend，that just basically fall back，to eager to test。

the robustness of the partial，guarded graph capture mechanism，we run it over 14k。

GitHub PyTorch models，with certain star levels，so it's not just trivial examples。

and our pass rate is like，99% and above，I didn't know，I didn't talk about integration。

to torch dynamo，but actually，that's a major design point，we really want different backend。

or accelerated compilers，to integrate easily，with torch dynamo，so I'm going to talk a little bit。

on the PyTorch native backend，we're building，that's the one here，but there are a lot of vendors。

since the PyTorch 2。0，announcement and release，are engaging with us。

almost all major backend compilers，are trying to integrate with us。

and we didn't put the exact number there，because it's changing。

and it also depends on the support levels，that will label it as experimental，or more stable ones。

and finally，we already actually show，the previous performance number。

but what we want to show is that，people might question，whether partial graph。

would sacrifice performance，for the backend compiler，what we observe is that，even though we are。

capturing partial graph，the graph is still large enough，for a lot of the optimizations。

to kick in like fusion optimizations，and because now we can，almost funnel any PyTorch compiler。

through the graph mode，so the widened coverage，actually more than compensated，on the smaller graph。

that the graph break is introducing，so we do see like，quite significant average speedups。

on a wide set of benchmarks，OK so this is Torch Dynamo，I want to remind people that，actually I think。

Albans talk is all about，like how do we optimize，the actually generate efficient code。

so keep in mind that，Torch Dynamo is the breakthrough point，for how 2。0 exists。

but by itself capturing graph，doesn't improve performance，we just enable the backend compiler。

to connect with PyTorch models，so that magic for improving performance。

in the PD-2 stack is Torch Dynamo，to characterize it，it's a PyTorch native compiler。

actually I should say，a PyTorch native training compiler。

because there are a lot of compilers out there，many are focusing on inference，and there are a lot of。

compilers out there，but few are designed for the PyTorch IR，and one of the reasons is because。

PyTorch IR is very，PyTorch offset is very large，and complex has mutation。

all these unpleasant features，that are convenient for，library developer。

but not convenient for compiler，so actually aiming for，IR design specifically for。

PyTorch semantics is very important，I don't have time to go into，the technical details on inductor。

but I just want to highlight a few things，that's unique in inductor，number one。

Torch inductor actually，most of the PD-2 stack，is written in Python itself。

and it allows us to move really fast，and make the things hackable，because nowadays。

a lot more people know about Python，than knowing about C or C++，and the second part is。

we started with，instead of focusing，resnet or a few models，to demonstrate the value。

we actually start with，focusing on designing the framework，to be general purpose。

to handle long tail ops first，and this is also very important，for handle real models。

so that's the best first approach，and the last one is the IR design，so I would say that。

if there's one thing，that you want to remember，about Torch inductor，the unique aspect would be。

the PyTorch IR native design，if you are interested in that，or extend on Torch inductor。

I would recommend you to，to look for more information online，so just to give you a mental map。

of what Torch inductor stack looks like，there are many boxes over here。

a lot of the compiler researchers，might be focusing on the code gen aspect，which is the hardest part。

and what's interesting for us，is being practical，we actually didn't do the code gen ourselves。

we actually rely on Triton，very early on，we are very early adopter of Triton，so one message here is。

like when you are building a practical，ML compiler stack from top to bottom。

you actually really want to，have the whole thing working，like the minimum viable product。

and there if you can leverage，other components，if they can do the job。

you don't have to build it yourself，so that's what we adopted，for the GPU code gen，we rely on Triton。

and that turned out to be a very，beneficial strategic bet，and then we also have a C++ backend。

right now we are involving，the Intel team，developing the C++ backend，especially for the CPU。

so then we are，I already talked about，like scheduling fusion，so these are what typical。

machine learning，optimizing compilers would do，so I wouldn't say anything about it。

the very unique thing，that make everything else work well，from PyTorch is actually our IR design。

so how do we lower，how do we design the IR，again don't have time to go into the detail，but I would。

if you were to design your own，machine learning compiler，to integrate with PyTorch。

I would recommend you to look at，how we are designing the，torch inductor IR，alright so。

I'm almost wrapping up my talk here，but I want you to take away，with this picture in mind。

left hand side going from eager mode，is the 1。2，1。x execution path，so this is eager mode execution。

basically one operation at a time，and 2。0 is the，torch。compile interface。

is actually completely backward，it's an opt-in interface，so it's completely backward。

compatible with 1。0，and what you will see is that，we are trying to funnel，the model with the same。

programming interface，into the graph mode，which is this part，as much as possible。

and the key technology in this，is actually this part，to make it possible。

because as I mentioned before，what made PyTorch beloved，made it really hard to，graph capture。

so once we have this component，figured out as an，outer box experience。

is almost like turning on a faucet，so the PyTorch models，just flow seamlessly，into the graph mode。

and torch inductor is the，the one that's developed in team，which is I would say。

is the best performing，training compiler for PyTorch today，but I also want you to remember that。

the role of a machine learning，firmware compiler is not to，build everything ourselves。

and because we do understand，like we have resources to invest，in say GPU。

but what about the other accelerators，so we do want to enable，other vendors to hook。

their own compilers with this part，we don't want them to worry about this，this is so hard and。

not very interesting for them，to figure out，so here there are many other，machine learning compilers。

this figure is a little bit，simplified because there are，actually different integration point。

for a machine learning，accelerator compiler to，interface with PyTorch，you could do it at this level。

like siblings of torch inductor，you could actually add backends，below torch inductor。

for example the Intel CPU backend，is something integration here，and increasingly we are seeing。

people actually integrating，at the Triton level，so for example we've seen，AMD is investing。

their GPU code gen into Triton，so if you are connected with Triton，this thing also flows naturally。

so I want you to keep in mind，the lower in the stack you integrate。

the more you can reuse what we build，and the less work for you，if that integration point。

is suitable for your hardware，all right so let's see，OK so I'm going to skip here。

but what I want to say，if I reflect back on 2。0，I also had research background，I think the one thing。

one theme is，we're trying to define，a machine learning framework compiler，that works for real models。

and we are trying to be，very very flexible，that's all we're trying to do。

and a lot of decisions is to，solve the hardest problems first，and then try to reuse。

as much component as possible，all right so what's available now，so if today you go to。

PyTorch GitHub 90 release，you would already have a lot of the，these are the core technologies。

that's already built in there，and you have Torch compile，as a compile mode API，or graph mode API。

it's a alpha feature，you have Torch inductor，as a default backend for GPU，you can use it for both。

inference and training，you have dynamic shape，as experimental feature，but you have to use it。

with a flag on，there's a lot of progress，made on dynamic shape since March。

and you can get the weekly updates，from the dev development，developer forum。

and we have three performance mode，for Torch compile，the default，reduce overhead is for models。

where there is a lot of kernel launch，so we made it work with，Cuda graph and other optimizations。

and max auto tune，this enables auto tuning，and what's coming next，so for the，maybe before the next。

PyTorch conference，which is in October this year，we are still continuing to，improve performance。

because we do believe，we are just scratching the surface，of what's possible。

and we are also improving，the composability with other，PyTorch features，for example quantization。

tensor subclassing，and so on and so forth，in the longer term，beyond six months。

actually we are already，doing a lot of work，on to PD to export，this is the pass，where it requires。

a little bit of manual effort，for you to export a whole model，with guards resolved。

into a savable form，and then you can carry this model，to whatever run time you have。

this is most recommended，for if you have like embedded devices，or inference in production。

where Python is not allowed，and yes we are talking about，large models right，so far I have more than。

talk about single GPU，so now we are also heavily，invested in PD to distribute it，and in order to not。

more than just optimizing for compute，we are trying to optimize，communication as well。

and I think Michael probably，will talk more about that，and to conclude，I just one message。

get involved，actually it's a tremendous effort，to build a machine learning，compiler by yourself。

so I would highly recommend，if your workload is using PyTorch，try to integrate。

at certain points with PD to stack，it really save you a lot of。

a lot of the work that's not necessary，and there is still a very，very strong momentum for us。

continue to involve PD to stack，so you can，and also when you try PD to，give us feedbacks。

if there is anything that's not working，or to be the direct，compute contributor to PD to stack。

especially for Python，and also for the，computer side，all right，thank you，[Applause]。

OK thank you Peng，OK so I think，we have two PyTorch，topics today，so that's why I。

we organized the local，deep learning framework topic，like PyTorch and MindSpot，into the session。

in the other AI open source forum，not in this one，OK let me introduce Michael Gashwin。

yeah he's another old friend，another old friend，he actually work for IBM，for a long time。

and he's the inventor of，the cell processor accelerator side，yeah and I think some of you。

might still remember，cell actually is the，almost the first general purpose，the multi-core processor。

but with the general purpose core，and also the acceleration，acceleration core。

to accelerate the workload，at that time including the gaming，those kind of things。

and many years after，now Michael he is now in meta，and leading the acceleration side。

for the PyTorch 2。0，I think so we can see the change，of the PyTorch，and it good for the。

very simple to use，very friendly to program，and now it more focus on the performance。

so that's why we also the，we're happy to have the Michael here，to share these kind of things。

for PyTorch 2。0，and to share his things with us，thank you，(掌聲)，thanks for the kind introduction。

so I'm excited to be here today，to talk about how we optimize，PyTorch 2。0。

to be the workhorse for generative AI，to be the environment of choice。

for developers in the generative AI field，if we look at generative AI。

we find that generative AI today，uses artificial intelligence models，that are able to generate。

new and original content，across a range of modalities，images， text and music。

and these models can produce，realistic and creative outputs，that have significant potential。

in a wide range of domains，including art， design， entertainment， etc。。

what we know about generative AI models today，is they are the largest neural network models in use。

and as a result they create，a unique set of requirements，the most common generative AI model today。

and the largest models are the large language models，which are advanced AI models。

that excel in generating human-like text responses，and they have proven incredibly powerful。

for understanding and generating natural language，and enabling models to interact more naturally with humans。

an example of this is ChatGPT，developed by OpenAI，and has been trained on a very extensive corpus of text data。

so if we look at the language model size，we find that over the last five years。

language models have grown in size，by five orders of magnitude，that's a factor of 10 every year。

my background is in chip design，and their factor of 2x every 18 months。

was considered breakneck speed，and it has transformed society and technology，as we all know。

 that's why we are here，compared up with 10x in a year for AI models。

what comes with that is also an incredible demand，for processing power and for new capabilities。

to take advantage of the hardware，and to make these large language models，affordable， deployable。

and have the necessary speed，to interact naturally with applications and with humans。

The second class of models are diffuser models，or latent diffusion models， LDMs。

and they excel in producing media output，in particular images， video and more。

they use latent diffusion processes，to create these high quality outputs，Two common examples。

 well-known examples，are DALI-2 and stable diffusion。

So when we look at all of these generative AI models today。

we find three key requirements to make them performant，to make them deployable。

 to make them affordable，First and foremost is speed and efficiency。

Because these generative AI models，are so computationally expensive，to both train and query。

So efficient training and inference，are fundamentally important。

for widespread adoption of these models，When we look at these models today。

we find that they are all transformer models，based on attention mechanisms。

So this is in particular a component，that we want to optimize for。

to improve model performance and efficiency，And finally， large language models。

large generative AI models are large，surprisingly or not，And that means that they need。

distributed computing infrastructure to train，to make training， make those necessary progress。

to make training affordable，And to fit all the parameters of a model。

into a device or a plurality of devices，that are interconnected using distribution technologies。

So how do we solve that？，Well， for speed and efficiency，Pang talked about Torch Compile。

That's the workhorse for accelerating models，big and small，And they offer a significant benefit。

to all of the generative AI models，We optimize transformers。

with accelerated transformer implementations，that we implement in PyTorch 2。0，And finally。

 for distributed processing，we have PyTorch distributed，with data parallel。

and fully sharded data parallel training，So let me turn to how we accelerate。

the PyTorch transformer API，the standard API for transformers in PyTorch。

So the PyTorch API in general，including the PyTorch transformer API。

are designed for flexibility and ease of use，That is what attracts developers to PyTorch。

Pang talked about that before，that flexibility， the ego mode execution。

and a number of options for each of the operators，to create all types of neural networks。

with a range of choices，whether that's doing normalization。

before the basic attention function or after，using different types of activation functions。

and so forth，What all of that brings，is a transformer implementation。

that requires many PyTorch operators to implement，and to control the computational complexity。

These are executed in sequence，and allowing for all the different options，that might be present。

So in a nutshell，the flexibility comes at the price of performance。

usability comes at the price of performance，With Torch compile，and with the accelerated transformers。

we can optimize transformer implementations，On the one hand we use fused kernels。

to combine multiple operations，and avoid materializing large arrays，large tensors。

that would introduce bottlenecks around memory access，One of the pains of existence。

of anybody who has tried to optimize models，for accelerators or CPUs for that matter。

and they combine many operations，they fold the soft gem into the matrix multiply。

they fold two matrix multiplies，into a single larger structure。

That way we can combine many of the operators，and implement them with a single fused loop kernel。

The second optimization that we rely on，is using a fast path architecture for inference。

where we optimize inference，for where we capture common inference patterns。

and use kernels that are optimized，to exploit variable input sequence length。

So if we look at the literature，how to accelerate attention computation，there's been steady progress。

starting with a paper in 2021，that's just two years ago，that talked about how self-attention。

can be implemented without needing，ON squared memory。

It introduced the concept of fusing attention kernels，to reduce memory usage，and then last year。

Try Tao and his colleagues from Stanford University，published a paper called，Flash Attention。

 Fast and Memory Efficient，Exact Attention with IO Awareness，That's based on these insights。

that Rob and Stott first published a year before，The team from Stanford University。

provided an optimized CUDA implementation，for this algorithm，and it beat out NVIDIA's entries。

for ML Perf Transformer training，last year in 2022 in June，And finally， MetaZone Fair Research Labs。

developed Xformas，which is similarly based on，the observations by Rob and Stott。

with respect to optimizing for memory usage，to speed up attention computation。

They released a package called Xformas，that provides fast and memory efficient。

attention computation functions，that are domain agnostic。

and are widely used in the research community，in vision， NLP and more。

So if we look at flash attention，This figure is taken from the flash attention paper。

You can see the basic approach of，computing attention，while reducing the memory usage。

Rather than materializing the end by end，matrix from computing QKT，The algorithm has an outer loop。

that loads sub blocks of the K and the B inputs，into SRAM，And then in an inner loop。

iterates over Q to compute，a subset of the final result，that is then written to HBM。

to the high bandwidth memory on GPU cards，And then it proceeds to the next block。

iterating again over the vector Q，the query vector of attention，The outcome is this huge。

 huge improvement of performance，The authors of the flash attention paper，report 7。

6x speed up over the attention implementation，in GPT-2，owing to folding all of these functions。

into a single kernel，And if you look at these functions，they all have in common。

that they take a large matrix as input，and produce another large matrix as output。

and thereby bottleneck on memory accesses，in each of the steps，Let me turn briefly to Xformas。

Xformas was developed by Fair，Meta's research lab for AI，And it consists of customizable blocks。

that are independent，and can be used without the generic boilerplate code，that comes with usual。

with a API，is provided by PyTorch，that offers a broad set of options and flexibility。

These components are domain agnostic，and depend on researchers to integrate them，and to combine them。

to use for vision NLP，and many other modalities，This is a research first effort。

research first package，that contains bleeding edge components。

that are not yet available in mainstream libraries，or that was the case last year。

And these components are built for efficiency，and a particular memory efficiency。

to use to give researchers high speed of iteration，It uses its own CUDA kernels where necessary。

and otherwise dispatches to libraries like Hotglass。

that are available in the open source community today，Here is a view of the speedups。

that can be obtained with Xformas，showing a speedup up to 4X for training。

and a significant improvement，up to 50% in memory efficiency。

So both FlashAttention and Xformas were new，based on papers published in 2021，and came out in 2022。

And to see the speed at which PyTorch develops，they launched in 2023 with PyTorch 2。0。

One of the advantages of using an open source framework，is the hackability。

 the pluggability of new ideas，and making PyTorch the go-to environment。

for researchers to integrate their own work，to have the most impact。

and make it available to the largest community of users，Tri Dao and his colleagues at Stanford。

had independently developed FlashAttention，and shortly before submitting the paper。

and the training results to the MLPerf Benchmark competition。

they approached Meta about integrating this capability，into PyTorch。

And so we worked both with the FlashAttention code，and its developers。

but also with the Xforma developers，to integrate these new bleeding edge libraries，into PyTorch。

and making them available out of the box，for all developers，We did that using a new operator。

Scaled-up product attention，It's not only a new operator，It's a new kind of PyTorch operator。

It's an operator that comes with multiple implementations。

that support different inputs and hardware models，So before in PyTorch you had one implementation。

of each operator per hardware platform，And that would cover the entire operating space。

This is different here，With scaled-up product attention，Depending on different hardware types。

Even different types of inputs，Sizes， dimensions， ratios， data types， etc。。

It dispatches to different implementations，They all have a common interface。

The underlying STPA operator，And the high-level STPA operator，Implements a selection logic。

That looks at the inputs，That considers the current hardware platform。

And figures out which implementation to call，This allows models to take advantage。

Of optimized implementations，Depending on what hardware they run on。

And at the same time makes models，Portable across different models and hardware types。

And finally we've integrated STPA，Into the PyTorch API for multi-headed attention，And transformers。

 both encoders and decoders，To accelerate training and inference，For new models。

But also for existing models，That were developed and even trained，Before PyTorch 2。0 was released。

With its support for accelerated transformers，It goes hand-in-hand with forward and backward。

Compatible weight formats，So you can train with the new operator，With the accelerated transformers。

And save the weights to use with the legacy model，If that is deployed someplace。

Or conversely you can use a previously trained model，And fine tune or deploy for production。

Using the accelerated transformer capabilities，So here is how the scaled product attention plays out。

The top level function is implementation agnostic，And then dispatches to one of three implementations。

This is for NVIDIA GPUs in particular，There is a fallback kernel STPA math。

That implements the mathematical description，Of the attention logic。

Calling internally a sequence of PyTorch operators，It is basically the legacy implementation。

And the fallback，If no better implementation is available，We have the STPA flash attention kernel。

That provides a CUDA implementation of flash，Attention for A100 and some other devices。

For 16-bit floating point data types，And certain input， common input sizes。

In cases where flash is available，It is typically the fastest choice。

And because it is highly optimized，For just these few operating conditions，And uses inline PTX。

And all sorts of other performance enhancements，And finally there is the STPA memory efficient kernel。

That comes from the Xformer library，That provides a CUDA implementation。

Of memory efficient attention，Developed by META's Fair Paris Research Lab。

We have another implementation，That is Triton based，That we can use in the future。

For hardware that cannot take advantage，Of the flash or the memory efficient implementations。

That implements a code generator，For the Triton kernels，So this chart shows。

Explores how to optimize large language models，With accelerated transformers，Using Andrej Karpaty's。

Open source nano GPD implementation as example，So step number 1。

If you want good performance and efficiency，Is always you enable Torch。compile。

There's really no excuse not to use it，As Peng highlighted，It has over 99% success rate。

Across a broad range，A very broad range of models，That are out there today，And then the next step is。

We replace the Python implementation，Of attention，With the scale。product attention operator。

That improves overflow handling，Because the integrated SDPA operator，Distributes how scaling works。

To avoid overflows，It delivers better run time，But memory efficient kernels。

Being faster than the math implementation，Of flash offering a further improvement。

And it delivers lower memory usage，Which allows you to use a bigger batch size。

Finally for large language models，Padding the vocabulary size，Helps with tensor core efficiency。

Because downstream matrix multiplies，Can take advantage，Can take better advantage of tensor core。

Tensor cores when vocabulary size is aligned，So we see the improvement here。

We start with a baseline of about，250 milliseconds per batch training time。

When we enable Torch Compile，It drops to 140-ish milliseconds，As we replace the legacy Python。

Attention implementation，That was in NanoGPT，But the different kernels。

That we have in accelerated transformers，We are able to get another significant reduction。

And then finally padding the vocabulary size，To take better advantage of tensor cores。

Offers yet another boost，The other thing that you see here is that。

With the memory efficient attention implementations。

That's both flash and the X-former implementation，You can use much larger batch sizes。

Because they make more efficient use of memory，And that in turn is another boost in performance。

If you look at this，Here the 2X speedup，This plots the validation loss after one hour of training。

So flash attention by offering a faster iteration time，Offers much better validation loss。

Regardless of batch size，And you can see the distance between the two curves。

Reflecting about 2X speedup，Between the Python baseline and the flash attention。

So then if we extrapolate that out，To the improved validation loss。

That we get from using larger batches，Using larger batches in essence translates to。

Another over 2X improvement in speedup，Of course that depends on specific convergence behavior。

Of your specific model，We work with Hugging Face to optimize decoder models。

And we found that we got up to 70% inference speedup，For diffuser models，And for training LLMs。

We got 70% training speed improvement，And about 20% inference speedup。

In addition we got over 110% memory savings in some instances，Plus more importantly。

The models did not oom for many cases，So accelerated transformers enabled new capabilities。

So in addition we have the fast path logic，And that for inference。

And the biggest benefit out of fast path，Is taking advantage of nested tensors。

These are a subclass of torched tensor，That Peng mentioned earlier。

And they allow to capture the variable length tensors，Inputs that are common in NLP processing。

And that can deliver up to 4X speedup on BERT style models，And up to 2。

5X inference speedup on vision transformers，So we have parallel training support，In PyTorch 2。0。

With both data parallel training，And fully sharded parallel training。

I'll skip that in the interest of time，And so I'll conclude with the summary，That PyTorch 2。

0 brings significant out of the box acceleration，To PyTorch transformer API。

And to models for training and inference，That use transformers，We've integrated that in Torch text。

 Torch vision，Torch multi-model， Fairseq，And we've worked with Hugging Face to integrate as well。

And by providing that library integration，We can get performance out of the box。

For a broad set of models，That were developed even before PyTorch 2。0，Finally PyTorch 2。

0 provides abstraction and portability，Using both Torch compile and the new STPA interface。

That allows models to take advantage of new improvements，That will come online both in the compiler。

And in new high performance kernels for transformers。

To deliver the best possible performance of models in the future。

Even on devices that are not available today，With that， thank you for your attention。

And I'll conclude my talk，(Applause)，Thank you， Michael。

And it's amazing that a lot of good stuff is coming into the PyTorch 2。0。

And not only to accelerating the inference side，But also important for the training side，Okay。

 so let me introduce our next speaker，Actually， Jing Gong， he came from AnyScale，AnyScale。

 I think people might have heard about Ray，That is another very good framework。

And especially for optimized distributed training side。

So today I'm very glad that I can invite Jing Gong。

He actually is leading the machine learning team at AnyScale。

And AnyScale is the company who is supporting the Ray，Very fast， the growth。

So let's welcome Jing Gong，(Applause)，Thank you，Thanks for the kind introduction too。

Very glad to be here talking about Ray with the audience today，My name is Jing。

 I lead the machine learning team at AnyScale，Which is the company that's behind the open source framework。

 Ray，So let me see if I can figure out how to use this thing，So I don't have the slides actually。

But fortunately Michael just showed the chart during the last talk。

The scale of the machine learning workloads these days。

It seems to be following some sort of exponential curve，And it seems to be actually accelerating。

Even by the large model generative AI stuff，So that's actually the reason we believe。

The folks at AnyScale believe why the framework of Ray，Catch a lot of attention and popularity。

Gaining a bit of popularity lately，When you're talking about open source frameworks。

People often use the GitHub star as a metric，For measuring how popular the framework is。

So here's a quick chart to show that，The red line is Ray，And we sort of have caught up to Kafka。

Since the last month in like half of the time，And we are marching towards the spark right now。

So a little bit of history of the framework，I think six， seven years ago。

The two founders of AnyScale，They were working on reinforcement learning at Berkeley。

The reinforcement learning at those days，Are a little bit different from today。

Where the computation is actually usually blocked，On the simulation side instead of the training side。

Because reinforcement learning is essentially，Randomly exploring in some simulated physical world。

And the founders actually just found themselves，Repeatedly stuck in this situation。

Where they need to write a lot of gRPC，And talk to all these remote simulated environment。

And collect the training sample，The actual training of the policy。

The reinforcement agent is actually pretty fast，So the computations are all blocked。

On the simulation side，So they thought let's write some framework，To make it easier to do gRPC。

And to do bandwidth efficient data movement，And that kind of stuff。

So this is how the framework starts，And this is also how why if you look at Ray。

RLM which is like a RL，Reinforcement learning library，That's built on top of Ray。

Is one of the first library，That's there since like day one，And it's a little bit unusual I'd say。

And then they quickly realized that，This framework seems to be useful。

For not just reinforcement learning，But other types of distributed computing as well。

And the abstraction seems to be nice and elegant，That's why they started the company AnySkill。

To try to commercialize Ray，And since the company is founded，We've added other libraries like Serve。

Which is to tackle the inference，And serving use case for machine learning。

And also last year we launched Ray Air，Which is sort of like this umbrella term。

To cover this end-to-end machine learning，Use case，And also like more recently。

There's actually also Ray dataset，Which I'm going to talk about later。

I didn't realize there's animation for this，So this is like a picture of the native ecosystem today。

So at the very bottom you have physical computation，Computing cluster and machines and all that。

And the Ray core sort of sits on top of the，The physical computing devices。

To extract away the infrastructure side of things，And above Ray core。

You're pretty much just dealing with native Python。

And you don't have to worry about GPUs or CPUs or memory，And all that kind of stuff。

And once you have Ray core，Then we build all these libraries，Ray train for deep learning。

Ray tune for hyper parameter tuning，And serve and dataset to cover the entire。

End-to-end machine learning workflow，Okay， so now that we have a high-level picture。

Of what the Ray is and what the ecosystem might look like，What I want to do is to get into。

Some of the more specific use cases of the whole ecosystem，I want to discuss the common pain points。

That we see from our day-to-day life，Dealing with customers and users。

And also explain a little bit of how the Ray libraries，Are trying to tackle those。

And try to make those things easier，The first thing is actually，So this is in the workflow order。

So the first thing I want to talk about，Is the data ingestion side of things，And we。

This is a little funny，We actually built this part the last。

This is the most recent addition to the ecosystem，Where we introduced the Ray dataset。

I think just yesterday somebody was asking me，Why do you guys want to build a dataset。

PyTorch has a data loader，Hacking phase data has a dataset，The answer was actually。

While we are dealing with helping users，And customers debugging their workload。

And dealing with these practical matters，You actually found the distributed data ingestion。

Is actually a little bit trickier than you think，Because these things normally，Runs on CPU。

Like the preprocessing and data loading，There's multiple steps between，You load the data。

And before they get to the GPU for training，So that makes the cluster usually heterogeneous。

Meaning you will have a different shape of the machine，You will have different resources available。

On different nodes of the machines，And also when you，When the scale of your data gets large。

Often times people will crash，If your data loader requires you to load，All the data into memories。

I was actually always a little bit suspicious，If somebody tells me。

I need to load all the data into memory at once，Usually those things don't work，And because of Ray。

And because of this shared memory space，On top of multiple machines，And within the cluster。

The team felt like we can build a library，To make these things a little better。

And another important thing is，If you actually go debug，People's machine learning workloads。

You will realize，A lot of times when they say，The GPU utilization is pretty low。

The problem is actually happening，On the ingestion side，Some sort of prefetching and stuff。

Will solve those cases，So that's why we built Ray dataset，Which provides a distributed。

And resource aware，Data transformation ingestion，Meaning if you have a cluster。

Where you have three GPUs，Doing the actual data parallel training，And you have 30 CPUs。

That's doing image reading and transformation，Before they get feed to GPU。

Datasets are actually aware of these resources，And will dispatch your work to the right node。

And then it's going to try to be smart，And take data locality into consideration。

And make the whole thing pretty efficient，And I think just last release。

We also enabled the streaming execution，As the default strategy，Meaning by default。

We'll never try to load your data，All into memory once，Everything is streamed。

From all the way from the source device，And we will apply proper back pressure。

So that your job is efficient，But the performance won't degrade，To like a blocking gesture basically。

So the next is batch inference，I mentioned this second，Although this is like the step。

After you get the model，I want to discuss this next，Because it's also handled by the dataset。

Instead of just doing ingestion，A common thing that once you've trained the model。

Is to actually use the model，And run it over a large set of data，This we call it batch inference。

You can think of it for example，Sometimes you have a large corpus，Of text data。

You want to run some embedding model，To turn them into embeddings。

That task actually has a very similar，Characteristic as the ingestion part。

Because you're still dealing with，A heterogeneous cluster，And the data loading will happen somewhere。

Like from S3 cloud storage，But the model needs to run on GPU，So that job is also handled。

We think the best by Ray dataset，And I'm going to show you，Actually some benchmark that we did。

More recently，We were comparing using Ray dataset，Versus using the SageMaker batch inference thing。

And also using Spark to do this batch inferencing job，SageMaker is actually really bad。

Because it's more like a wrapper，Around this online serving system。

Which is totally not fit for offline model inferencing，The more interesting part here。

Is the comparison between Ray dataset and Spark，The team actually spent a lot of time。

Optimizing the Spark job，To make sure we're doing a fair benchmark comparison。

And in the end we were able to，Roughly outperform maybe 3X，Compare with a multi-node Spark cluster。

The reason behind that is because，Spark constantly switches between Python and Java。

Like these two words，So your data gets serialized and deserialized，Really often between them。

And also I think Spark has，Less flexible processing fusing，So they try to over-fusing。

Multiple steps together，And if you have one step in between that，Like bottleneck on GPU。

Then all your CPU steps are slower as well，That's where Ray tasks shines。

And give you some extra performance，The funny thing about this is also。

When we published this benchmark，The folks at Databricks actually pinged us and said。

We can optimize this better，This is not fair，And they spent some time，And in the end they admitted。

Yeah， we can do this better than dataset，So about distributed training and tuning。

This is like bread and butter，For any machine learning systems。

What we notice about distributed training is that，These days the ecosystem for training。

Is extremely fragmented，Meaning you just get all kinds of frameworks，That you can do this。

And also things move so fast，It's very hard to say which framework will win。

The industry is completely not consolidated，Onto any particular framework，Especially like these days。

You start to use deep speed，Or like accelerate to do large model training。

Which totally wasn't the case three months ago，So what we felt like we should do for distributed training。

Is we want to make the options open，We want to make the system very extensible。

So users don't get locked in，To some single framework。

That they end up having to switch with a lot of effort，So the Raytron library is rather lightweight。

It basically provides a lot of integrations，With the existing training frameworks out there。

Like PyTorch Lightning or Deep-C，Or Hugging Face， etc。。

What we take care of is mostly the setting up the cluster。

Setting up the runtime environment part of it，The actual deep learning and the distributed communication。

Is handled by the underlying library，That basically allows these machine learning engineers。

And data scientists to do this，Without worrying about the hardware，As long as you have a Ray cluster。

Your job can be up and running，And also we made it so that，It's very easy to turn a training job。

Into a hyper-perpendicular tuning job，Because Raytuning is actually orchestrating。

All the work behind this in many ways，So you just need to switch a few lines of Python code。

To do that，Raytuning for some reason，Is one of the most popular libraries that Ray has。

So about serving，We talked about the offline batch inference，So in terms of online serving。

What we see the users and customer want，Are auto scaling，Because the GPUs are really expensive。

And the serving traffic is actually naturally cyclic，So they want to scale down to very few nodes。

During the night and scale up quickly，So the auto scaling speed is actually very crucial。

For any practical serving system，And they also want fault tolerance。

So we basically put in a lot of work，Introduced highly available serving，Maybe since last year。

And now you get to enjoy，Highly available cluster，And the Ray serve deployment，With relative ease。

One thing I also want to mention is that，From day one actually，The selling point of Ray serve。

Is because it's built on top of the Ray core，And the Ray has this actor concept。

Where you can write a little microservice，Using simple like Python class。

And that thing is actually backing，All these model replicas。

It made it very easy to do model composition，And for a lot of the use cases。

Actually it's not about a single model，You want to score something，Using a set of models。

And you can do it in a graph fashion，That actually is a very，It's like a pretty sweet spot for serve。

And the reason for a lot of people to use it，And of course more recently，Because of the large models。

We introduced some features，That's not important before，Such as like streaming support。

And we had to like improve，The large model，Just to be able to scale up，The large model deployment。

Cool， so the last slide is about，Generating AI，I think the talk these days，Will not be complete。

If you don't mention large model，The Ray's interaction with the，Large model and generating AI world。

Is actually pretty early，Even before I joined，Maybe roughly around，When I joined any scale。

We were working with this open source team，They were trying to train this model called GPT-J。

Which is a replica，Tried to open source replica of GPT-3，I think。

And they got a lot of free credit from Google，So they were training those things on TPU。

And we were helping them，To set up a Ray cluster on TPU，So they can have a reasonable。

Developer experience，Instead of dealing with like a large，Cluster of TPU instances。

So that's when this thing started，We actually didn't pay too much，Attention about this。

Until very recently when like Databricks，They fine-tuned Dolly on top of GPT-J。

Everyone was surprised，Like actually the model is pretty good，It's better than you think。

And other people who are using，Ray to do large model including，Cohere AI，If you guys don't know。

The founders of Cohere，Are the inventors of the transformer structure。

And also we recently got permission，From OpenAI to say，They actually use the Ray core，To train GPT-4。

So from our perspective，Large model is not like a single，Kind of thing，Because there are model sizes。

That you can do with a single instance，As long as you have a few GPU cores，On the instance。

You can do a reasonable sized，Large model training on that，So from our perspective。

We try to support the work，For our customers and the open source。

Communities by offering a spectrum of things，For example if you are okay with。

One compute node and eight GPU cores，Then we have these off the shelf examples。

That you can just quickly run，And your job will be up and running in no time。

And if you are trying to run，These relatively smaller scale，Large model training。

Then you can use the ecosystem，The native ecosystem library，And we take care of the hardware。

And you can train your job，And if you are really really hard core，Like Able AI and Co here。

Who is trying to write，Your own custom training stack，And optimize everything yourself。

Then they actually use，They don't use any libraries，But they use a Ray core。

To help with the developer，And experience and also the iteration speed，So that's what I mentioned。

I think it's a useful thing to share，At this conference，Okay so when I was writing these slides。

I was looking at some of our，Biggest open source partners and customers。

To see what are the most common use cases，They have for Ray，Aside from the big model stuff。

Surprisingly actually the most common use cases，They will build an internal Ray cluster。

As their machine learning platform，So that their data scientists，And machine learning engineers。

Can basically just submit the jobs，To this cluster and they get the result，Without doing any kind of。

Kubernetes stuff or some，Because the data scientists，They get to interact with Python。

Which is the thing that they use everyday，And super familiar。

That helps with their developer experience a lot，We obviously，So I don't have time to mention。

But our lib is sort of this，The only off the shelf choice，If you want to do。

Production scale reinforcement learning，And we actually have a pretty big，Users of the library there。

And obviously Ant，Is actually a very big adopter of Ray，From day one。

And they are sort of our colleague，They submitted our PRs to the code base，Lastly I would encourage。

Ray is completely open source，You can please take a look at our GitHub。

And also the forum and try to get involved，We welcome all the contributions from，The community。

The company is actually relatively small，We have very limited resources，So a lot of times。

The awesome things happen，Because of the community，That's it，[Applause]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]，[English]。

[English]，[English]，[English]，[English]，[English]，拜拜。