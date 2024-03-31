* out of the gate didn't know what model to use
* picked up llama 2 because I'd heard it was the most performant of the smaller local models, didn't spend a long time researching this
* I was also doing this on a new machine. I changed how I work with python for this. pyenv
* lesson one: bloat builds up fast and it is easy to be swimming in used up disk space
    * the models are huge
    * there are a lot of python packages involved in this
        * see if you can provide the exact list of dependencies you ended up needing
        * get numbers for disk space and explanation, maybe for footnotes
* lesson two: don't bring scuba gear to trench, know what you are willing to go deep on when. 
* talk about starting with the readme on hugging face and their examples and running into issues because you didn't understand model terminologies, and then talk about how you were able to get running faster with ollama and langchain
    * didn't know what to do but I'd heard llama was good. searching led me to know that llama 2 was performing the best next to the big models that openai and google and anthropic were putting out. 
    * documentation said to get llama from Meta, the model should come from there, you have to sign a license agreement about usage and they want to track who is using them
    * I did this first, and had a huge model now sitting on my hard disk
    * I started looking around for an easy way to chat with it. I found facebook's repo first, llama repo. its instructions (review and report how you followed htings and what pitfalls you had.)
    * the facebook repo points to the hugging face llama recipes repo for doing more with the models. 
    * the llama-recipes github repo has you converting your model using a script to have hugging face checkpoints. they say all models on their site use this
    * I then attempted to follow their rag tutorial and got stuck somewhere
    * I then just tried to follow hello llama local tutorial which required conversion of the model to gguf format. after doing this and possibly the hugging face conversion for some reason the inference was extremely slow on my machine, which is not a slow machine. 
    * I put in an issue to the llama recipes repo about it. They ahve been following up with me since to try and look into it, but generally advised it was slower inference. 
    * While I was waiting for the hugging face team to respond, I decided to remove the old llama2 model from my computer and then switch to using ollama to download and manage the model and a langchain script to use it with somethign like RAG, maybe exactly RAG.
    * I don't remember my headaches with this part, there were fewer of them. ollama worked pretty seamlessly but let me try and remember what the download process was like. I think I got it with brew. then there is a cli to install the model. that I remember being a little bit hard to find docs for. I remember wondering how to use the model once I downloaded it because it was just running on localhost. langchain wraps ollama's api so it wasn't so bad once I figured it out, and it was easy to add a text splitter and move forward. 
    * ideas for things I want to build with this simple approach: 
        * an email classifier, and ultimately an agent to reorganize my emails and tell me what it did.
        * something to summarize large language model work
        * something to help me build my practice app in scala in such a way that I learn what I'm doing as I do it.
    * quantization, what you learned about that. 
    *  