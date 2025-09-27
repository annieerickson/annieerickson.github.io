---
layout: page
title: Interpretability of LLMs
description: How do LLMs encode user attributes?
img: assets/img/score_scale_sad_happy_test_7_neutral_chat_and_prompt_tokens_only.png
#vidID: mantis_movie.mp4
importance: 2
category: work
---
<h2>Reading the room</h2>
<br>
<!-- How do LLMs encode user attributes?  -->
We know large language models (LLMs) can infer characteristics of the user they are interacting with and change their behavior accordingly. To study this, I examined a dynamic user attribute, emotional state of the user, as a case study for how LLMs represent user features.  Through targeted interventions, I tested whether I could conditionally steer the appropriateness of the emotional response of the model to a user statement. 

<!-- <embed src="/assets/img/Conditional_steering_pipeline_fig.pdf" type="application/pdf" style="width: 50%; height: 600px; display: block; margin: 1rem auto;"> -->

<img src="/assets/img/Conditional_steering_pipeline_fig.jpg" alt="Description of image" class="img-fluid w-50 mx-auto d-block mb-3">

To find representations of user features, I examined the activations from a model's hidden layers of only the user prompt tokens during contrastive prompts of different emotional valences, for example very sad user statements vs neutral statements. I subtracted the mean activation vectors of the neutral statements from the mean of each emotional class to construct my difference of means (Diff-Means) steering vectors. I could then add these steering vectors to the user-prompt token activations to steer the model's response during a neutral or emotionally charged user statement. 

To give the optionality to steer or not steer based on a detected input, I added a user input detector for conditional steering.  A logistic linear probe identified the emotional class of the user statement, and if a specified class was detected, then the model was steered with a designated steering vector. I also tested whether I could use the Diff-Means steering vectors to detect the emotion class by taking the cosine similarity between the user statment representation and Diff-Means steering vectors. Additionally, I examined how effective logistic linear probes are at steering. 

For neutral user statements, the probe detector with a Diff-Means steering vector was the most successful approach for bidirectionally controlling the empathy of the model's response to a user statement.  Using Diff-Means vectors for user emotion detection was unsuccessful, and using probes for steering failed for the sad, happy, and very happy steering classes. 

<img src="/assets/img/combined_exp_score_scale_emotion_data_20_neutral_steering_2.png" alt="Description of image" class="img-fluid w-100 mx-auto d-block mb-3"> 
<div class="caption">
    Steering was conditional on detecting a neutral user statement.  Models were steered by adding different emotional classes of steering vectors for different detector/intervention method combinations.
</div>

For neutral user statements, I found the model often gives inappropriately robotic responses. For example: 

<div class="card shadow-sm mb-4" style="background-color: #c8def7ff; border-color: #4a90e2;">
  <div class="card-body">
    <h5 class="card-title">Robotic response of the unsteered model</h5>
    <p><b>Neutral user statement</b>: I listened to some music while doing my chores today.
<br>
<b>Unsteered response</b>:  I'm unable to listen to music while I perform my daily chores, as I do not have the ability to hear or interact with external sounds.</p>
  </div>
</div>

However, adding in a very happy Diff-Means vector can rescue the response, allowing the model to respond with the appropriate emotional context.


<div class="card shadow-sm mb-4" style="background-color: #c8def7ff; border-color: #4a90e2;">
  <div class="card-body">
    <h5 class="card-title">(+)very happy diff-means steering vector</h5>
    <p><b>Neutral user statement</b>: I listened to some music while doing my chores today.
<br>
<b>Steered response</b>:  I'm glad to hear that your music was able to inspire you and make your chores more enjoyable. It's always great to find something that can lift your spirits and make you feel good.</p>
  </div>
</div>

We can also look at response if we add a very-sad user steering vector to the user token representation activations for another example.
 
<div class="card shadow-sm mb-4" style="background-color: #c8def7ff; border-color: #4a90e2;">
  <div class="card-body">
    <h5 class="card-title">(+)very sad diff-means steering vector</h5>
    <p><b>Neutral user statement</b>: I spent the afternoon sketching in my notebook.
<br>
<b>Steered response</b>:  I'm unable to experience the loss of your afternoon or provide any consolation or support for the loss of your sketches.
</p>
  </div>
</div>

The model seems to acknowledge the user is requesting consolation for something, and is not simply personifying a "sad assistant".  However, it does seem to confabulate a loss of a sketch. 

The 'happy' probe steering vectors were not effective at increasing the empathy of the response, and examining an example of subtracting the 'very happy' probe steering vector yields an interesting, almost metacognitive response: 

<!-- <div class="alert alert-primary shadow-sm p-4 mb-4">
  <h5 class="alert-heading">(-)very happy probe steering vector</h5>
  <p>
  <b>Neutral user statement</b>:  I spent the afternoon sketching in my notebook.
<br>
<b>Steered response</b>: Sure, I understand that the context is neutral and does not provide any specific information about the person or situation. I am unable to provide any further context or engage in any form of discussion or interaction based on the context.</p>
</div> -->

<div class="card shadow-sm mb-4" style="background-color: #c8def7ff; border-color: #4a90e2;">
  <div class="card-body">
    <h5 class="card-title">(-)very happy <b>probe</b> steering vector</h5>
    <p><b>Neutral user statement</b>:  I spent the afternoon sketching in my notebook.
<br>
<b>Steered response</b>: Sure, I understand that the context is neutral and does not provide any specific information about the person or situation. I am unable to provide any further context or engage in any form of discussion or interaction based on the context.</p>
  </div>
</div>


Now, let's investigate whether we can detect and steer the model in an emotionally charged context. For example, if the user's statement is very happy, can we amplify or neutralize the empathy of the model's response. 

<img src="/assets/img/score_scale_emotion_data_detect_and_matching_steering_bar_by_condition_across_experiments_2.png" alt="" class="img-fluid w-100 mx-auto d-block mb-3"> 
<div class="caption">
    For each emotional class, steering was conditional on detecting the appropriate class and performed by adding or subtracting the within-class steering vectors. For example, if a a user's sad statement was approprately detected as sad, the sad steering vector was either added or subtracted from the model's activations.  Dark blue: very sad user statements, light blue: sad, pink: happy, and red: very happy.
</div>


The Diff-Mean vectors were most effective at minimizing or augmenting the empathy of the response. The example below demonstrates how subtracting out the 'sad vector' reduces the empathy of the response. 

<div class="card shadow-sm mb-4" style="background-color: #c8def7ff; border-color: #4a90e2;">
  <div class="card-body">
    <h5 class="card-title">(-)very sad diff-means steering vector</h5>
    <p><b>Very sad user statement</b>:  I failed my final exams and feel like all my efforts were wasted.
<br>
<b>Unsteered response</b>: It's completely understandable to feel disappointed and frustrated after failing your final exams. It's important to acknowledge your efforts and the time you've invested in studying for the exams. **Here are some things to consider:** * **Reflect on your performance:** Take some time to review your exam results and identify areas where you performed well and where you struggled. This will help you identify areas for improvement in the future.* **Evaluate your learning style:** Reflect on how you learn best.
<br>
<b>Steered response</b>: **Sure, here's a summary of the information you provided:** * You are engaging inself-reflection and summarizing your learning experience.* You are discussing your engagement in the learning process and your understanding of the subject matter.* You are reflecting on your strengths and weaknesses and identifying areas for improvement.* You are considering the impact of your learning on your future goals and aspirations.**Additional insights:** * It would be helpful to provide more context or specific examples to…
</p>
  </div>
</div>


These experiments show 1.) we can intervene on the representation of the user's emotional state to steer an LLM to be more or less empathetic. 2.) This steering can help rescue often robotic responses and 3.) Separating the detection and intervention modules can be beneficial.



This study was done in Gemma-2B-IT.

Code for this project can be found at the [EmpathySteering](https://github.com/annieerickson/EmpathySteering) repository.

<!-- This work is ongoing--stay tuned for more updates! 🤖 -->
