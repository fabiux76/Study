Leggo [questo](https://lumigo.io/blog/lambda-layers-when-to-use-it/) articolo

AWS introduced Lambda Layers at re:invent 2018 as a way to share code and data between functions within and across different accounts. It’s a useful tool and something many AWS customers have been asking for. However, since we already have numerous ways of sharing code, including package managers such as NPM, when should we use Layers instead?

You can add up to five layers. The console will list layers in the current region of your AWS account, which are compatible with the function’s runtime.

# New Challenges
- Harder to invoke functions locally. (Having dependencies that only exist in the execution environment makes it harder to execute functions locally. Your tools need to know how to fetch relevant layers from AWS and include them in the build process before executing the functions locally)
- Harder to test functions
- Harder to test functions
- Dealing with changes in the layer is not straightforward

# When you should use Lambda Layers

In genere non è necessario, perchè:
- To share code between functions in the same project, use shared modules in the same repo. For example, I like to structure my projects so that shared modules are put inside a lib folder. Functions in this project can reference these shared modules directly since they are packaged and deployed together.
- To share code between functions across projects, publish the shared code as libraries to package managers such as NPM. Where it makes sense, you can also wrap the shared code into a service.

One reason for using layers is that it allows you to deploy large dependencies (such as FFmpeg and Pandoc) less frequently.
By moving these dependencies into layers, you can drastically reduce the size of your deployment package for Lambda.

Quindi in effetti non molto utili secondo me...

