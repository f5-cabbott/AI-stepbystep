![Static Badge](https://img.shields.io/badge/Author-f5--rahm-blue?link=https%3A%2F%2Fgithub.com%2Ff5-rahm)

---
# Installing Ollama on MacOS

1. Grab the app archive from the [Ollama download page](https://ollama.com/download/mac).
2. Unzip the archive once downloaded, then drag the app to your Applications folder.
3. Open the app and follow the prompts in the setup wizard to complete the installation.

# Run a Model

1. Start the terminal
2. Run a model. If you don't have it yet, it'll first download then run the model.
`ollama run gemma3:1b`
3. Let's test it out with a picture analysis. My test is a picture featuring dueling force-wielding cats. Here's the start of the output from the gemma:1b model, which returned immediately:
`ollama run gemma3:1b`                                                                                        
```>>> what can you tell me about this picture? /Users/Keanu/AI-stepbystep/lab1/dueling-cats-2.png
Okay, let's break down what we can gather about the image you provided:

**Overall Impression:**

The image is a highly stylized, almost cartoonish, depiction of a cat and a dog engaged in a challenging and playful "duel." It's a visual metaphor for a struggle of opposing forces or a competition.  The style leans 
heavily into a slightly exaggerated, slightly dreamlike aesthetic.
```
Close Ollama by typing `/bye`. 

One of the cats is apparently a dog. Per <a href="https://ai.google.dev/gemma/docs/core">Google</a> "The Gemma3 1B size models are text only and *do not support image input*. The model was unable to fully understand the picture and what we experienced was a form of AI <a href="https://www.ibm.com/think/topics/ai-hallucinations">hallucination</a>. 

To increase accuracy let's step it up to the gemma:4b model.
*Note: This will take longer as it's a larger LLM and we're running on non-AI optimized hardware.*

5. Type `ollama run gemma3:4b`, and then after the download completes, re-enter the same prompt as before.
```>>> what can you tell me about this picture? /Users/Keanu/AI-stepbystep/lab1/dueling-cats-2.png
Okay, let's break down this wonderfully surreal image! 

**What it depicts:**

This is a digitally manipulated image depicting two orange tabby cats engaged in a lightsaber duel against a backdrop of a bright, cloud-filled sky with a prominent rainbow.  Each cat is wielding a red lightsaber, 
and beams of red light are shooting between them.
```
This took about 30 seconds to process, but far more accurate to my picture!

# Bonus challenge

When running ollama on your laptop, it runs an http service on localhost port 11434, so you can send queries to ollama via another service. Let's try that!
<pre>curl http://localhost:11434/api/generate -d \
'{
  "model": "gemma3:4b",
  "prompt": "what can you tell me about this picture? /Users/Keanu/AI-stepbystep/lab1/dueling-cats-2.png"
}'
</pre>

Huzzah! That works. But...the output is not, uh, great:

<pre>
{"model":"gemma3:4b","created_at":"2025-04-14T22:13:44.058712Z","response":"Okay","done":false}
{"model":"gemma3:4b","created_at":"2025-04-14T22:13:44.08365Z","response":",","done":false}
{"model":"gemma3:4b","created_at":"2025-04-14T22:13:44.110131Z","response":" let","done":false}
{"model":"gemma3:4b","created_at":"2025-04-14T22:13:44.135686Z","response":"'","done":false}
{"model":"gemma3:4b","created_at":"2025-04-14T22:13:44.162662Z","response":"s","done":false}
{"model":"gemma3:4b","created_at":"2025-04-14T22:13:44.188051Z","response":" analyze","done":false}
</pre>

Ollama streams the response word-by-word by default. To fix this, we can set stream to false:

<pre>curl http://localhost:11434/api/generate -d \
'{
  "model": "gemma3:4b",
  "prompt": "what can you tell me about this picture? /Users/Keanu/AI-stepbystep/lab1/dueling-cats-2.png",
  "stream": false
}'
</pre>

That looks better, not great, but better. Remember we're calling the API so it's not meant for normal consumption but at least we can distil the output easier.

# Conclusion

Ollama is the foundation for many AI tools you'll use in later labs. Now you have your first model is running - boom, you're officially in the AI game! - Play around and get comfortable with the basics, then explore and see what's next!

