

It's very often that while studying mathematics you come across something very beautiful and amazing. It seems intimidating at first but when you start to understand it, you are only left with amazement and the thirst to learn more.   

Let's say that there are a few things on our table. We'll look at something that stands out or we'll give our attention to something that we want(like our phone). So, attention are basically of two types:

- Something that already stands out (nonvolitional cues) [*keys*]
- Something that we bias over cognitively(volitional cues) [*query*]

And we also have our own sensory inputs [*values*]

Now, what needs to be done is to make a selection over our sensory inputs(*values*) on the basis of those two factors; *keys* and *query* . This mechanism is called as attention pooling. 

To do the attention pooling, let's consider Nadarya-Watson Kernel Regression: 
$$
f(x) = \sum_{i = 1}^{n}{\frac{K(x-x_i)}{\sum_{j = 1}^n{k(x-x_j)}}y_i}
$$


This looks pretty intimidating, so generalizing this to a simple aggregate function:
$$
f(x) = \sum_{i = 1}^n{\alpha (x-x_i)y_i}
$$
Where,<br>																				$x = query$ and $$(x_i, y_i)$$ = key, value pair

Considering a gaussian kernel, the equation becomes: 
$$
f(x) = \sum _{i = 1}^n{softmax(-\frac{1}{2}(x-x_i)^2)y_i}
$$
Just by using this equation, a pretty good prediction is done: 

![/image/content/attention/normal.png]()



<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210626214749931.png" alt="image-20210626214749931" style="zoom:80%;" />![image-20210626214848706](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210626214848706.png)

This is amazing and now, just add a weight in the equation and train it like a neural network layer. 
$$
f(x) = \sum _{i = 1}^n{softmax(-\frac{1}{2}((x-x_i)w)^2)y_i}
$$
<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210626214749931.png" alt="image-20210626214749931" style="zoom:80%;" />![image-20210626214848706](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210626214848706.png)

My thinking of neural networks was pretty confined before I studied this equation. Just a non-linear function and train it; this has given a greater essense to that non-linear function. It has opened up many probabilities for me: how could I visualize real world better? We have so many activities going on in our daily lifes and just the act of paying attention is this interesting and amazing. I can only wonder about all the actual things.





**I will be trying to explain some of the internal stuff going on here which I'm not sure I understand them properly and correctly. So, feel free to send me a message if I miss something or write something completely wrong.**



### Attention Pooling 

For instance, let's say we have a database and we want to retrieve something. We give a query and based on a key, a value is retrieved, pretty simple but we can't exactly do that here. We mimic the method but in a more fuzzy/probabilistic way by creating a neural network architecture. 

So, we could say
$$
attention(query, key, value) = \sum_i{similarity(query,key) \times value}
$$
 The query we have is a single query but we have many keys and values. In the case of database, we'd have a discrete value of key that would correspond to a query but in the case of attentions it is better to visualize it more as a probability distribution(the similarity between queries and keys). So now, for a query, we will have a probability distribution of keys. 



**Still, how do we find the similarity?**

In the above example we'd used a kernel function, but that's not the only thing we could use. We could use any function(even a simple dot product between query and keys) to find the similarity between the two. After using the similarity function, we use the softmax function(well probabilities need to sum upto 1) and multiply them with their corresponding values. At the end, we just need to sum them and we've got our attention. 

#### Similarity function

There are many choices for the similarity function but in our case, we want to embed this into a neural network. So we use a weight 'w'. 

**But what exactly does the weight w learn?**

For simplicity let's take an example of a general dot product as a similarity function.
$$
f(x,x_i) = x^TWk_i
$$
In this case, both query and keys are embedding vectors. Query could be embedding a question and key could be embedding an answer. The embedding space of both the vectors might not be the same. The $W$ term essentially projects one embedding into another and using backpropagation, we find the value of $W$.



The <a href = "https://colab.research.google.com/drive/1yNVDctnsA-jkDlz6QtiVkPTmd5yFPr3a?usp=sharing">google colab </a> file for the code.  