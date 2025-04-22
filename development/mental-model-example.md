# Mental model: Practical example

## Scenario

We want to use an LLM to help a marketing team quickly generate draft posts for various social media platforms (e.g., Twitter/X, LinkedIn) based on a newly published company blog post or press release.

---

### 1. Understand the requirements

* **Task Familiarity:** Let's assume I (the developer) understand the *basics* of social media but am not a professional social media manager. I know different platforms have different styles and length constraints.
* **Ask Product Team/Marketing:** What are the primary platforms we target (e.g., Twitter/X, LinkedIn, maybe Facebook)? What's the desired tone for each (e.g., informal/engaging for Twitter, professional/insightful for LinkedIn)? Who is the target audience for our blog/posts? What is the *goal* of these posts (e.g., drive traffic to the blog, spark discussion, build brand awareness)?
* **Request Examples:** Ask the marketing team for examples of *successful* past social media posts they've created based on blog content. Get examples for each target platform. Also, get a few typical blog posts/announcements that would be used as input.
* **Expert Needed?** Yes, the company's social media manager(s) or marketing content creators are the experts. Their input on tone, platform nuances, and what resonates with the audience is crucial.
* **Human-in-the-loop?** Definitely. The LLM generates *drafts*. An expert *must* review, edit (for tone, hashtags, platform specifics, adding images/links), and approve posts before publishing.
* **Time Estimate (Development):** What is the estimated development time to create a working prototype? Aiming for a tool that significantly reduces the time spent drafting initial versions of posts for multiple platforms. A working prototype within a few days seems feasible, but this needs to be tracked against actual development progress.

*(Transition: Now that we understand the context and need expert input, let's define what makes a 'good' draft.)*

---

### 2. Set clear success criteria

* **How does good output look?** Drafts that accurately reflect the core message of the source text (blog post), are tailored to the specific platform's style and length constraints (e.g., short and punchy for Twitter, slightly longer/more formal for LinkedIn), use an appropriate tone, include relevant keywords/concepts from the source, and suggest relevant hashtags.
* **How does somewhat good output look?** Captures the main idea but might need significant editing for tone or platform suitability. Hashtags might be generic or missing. Requires noticeable work but still faster than starting from scratch.
* **How does bad output look?** Misrepresents the source text, uses the wrong tone, ignores platform constraints (e.g., too long for Twitter), includes irrelevant hashtags, or sounds generic/robotic. Requires a complete rewrite.
* **Minimum Quality:** "Somewhat Good" – the drafts must be a clear time-saver for the social media manager.
* **Success Criteria Scorecard (Example per platform draft):**
  * Accurate reflection of source text's core message: /5
  * Adherence to platform constraints (length, style): /5
  * Appropriate tone for the platform/brand: /4
  * Inclusion of relevant keywords/concepts: /3
  * Relevance and quality of suggested hashtags: /3
  * **Total: /20. Success Threshold: >= 15/20** (To be validated with the marketing team).
* **Alignment:** Share this scorecard and examples of good/bad drafts (based on Step 1 examples) with the social media experts for feedback and agreement.

*(Transition: With success defined, let's consider how a human tackles this task.)*

---

### 3. Outline human approach

How would a Social Media Manager do this manually?

1. Read the blog post/announcement thoroughly to grasp the key message, main points, and target audience.
2. Identify the most compelling angle(s) or takeaway(s) suitable for sharing on social media.
3. **For Twitter/X:** Draft a concise (under 280 chars), engaging message summarizing the key takeaway. Identify 2-3 relevant, trending (if applicable) hashtags. Add a link to the blog post.
4. **For LinkedIn:** Draft a slightly longer, more professional post. Focus on insights, implications, or questions for the professional audience. Maybe use a key quote or statistic from the post. Identify 2-4 relevant professional hashtags. Add a link to the blog post.
5. (Repeat for other platforms like Facebook, adjusting tone and format).
6. Review all drafts for clarity, brand voice consistency, grammar, and accuracy.
7. Schedule the posts using a social media management tool.

* **Refinement:** The core tasks are: 1. Understand Source. 2. Extract Key Point(s). 3. Adapt Key Point(s) to Platform Format/Tone. 4. Add Platform Elements (hashtags, links). This seems reasonably contained for an LLM, though tailoring to different platforms simultaneously adds complexity.

*(Transition: Understanding the human process helps select the right starting AI tool.)*

---

### 4. Pick an initial LLM model to start with

* **Task Complexity:** Medium. Requires understanding the source text, summarizing, adapting tone and format for *multiple* platforms, and suggesting relevant hashtags. Some creativity/engagement is needed, and reliable instruction following across platforms is key.
* **Model Landscape (Recent Options):** The AI landscape evolves quickly. As of mid-2025, key players include:
  * **Google:** Gemini family (e.g., 2.5 Pro for complex reasoning/coding, 2.5 Flash for speed/cost balance on complex tasks, 1.5 Pro for strong reasoning, 1.5 Flash/2.0 Flash-Lite for efficiency) [\[ai.google\]].
  * **OpenAI:** GPT-4 series (e.g., GPT-4.1 family focused on coding/instruction following, GPT-4o as a strong generalist, consumer models like o3/o4-Mini) [\[techcrunch.com, hindustantimes.com\]].
  * **Anthropic:** Claude series (e.g., Claude 3.7 Sonnet noted for strong performance, particularly in areas like coding) [\[techcrunch.com\]].
* **Model Choice Factors for *This Task*:**
  * *Quality vs. Cost:* Needs good summarization, tone adaptation, and instruction following. A highly capable model (like Gemini 2.5 Pro, GPT-4.1, Claude 3.7 Sonnet) might provide the best initial quality but could be more expensive. A balanced model (like Gemini 1.5 Pro/2.5 Flash, GPT-4.1 mini, Claude 3 Sonnet) might offer a good starting point.
  * *Tokens:* Blog posts can vary in length. Models with larger context windows (many recent models offer 1M tokens, e.g., Gemini 2.5, GPT-4.1) provide flexibility, though may not be strictly necessary if summarizing first is an option. Output length is short.
  * *Tools:* No external tools needed initially.
  * *Instruction Following:* Crucial for generating distinct outputs per platform. Newer models often emphasize improved instruction adherence.
* **Initial Choice:** Given the need for reliable instruction following across platforms and good summarization/adaptation, let's start with **Gemini 2.5 Flash**. It's positioned as offering a good balance of speed, cost, and capability for complex tasks, and has a large context window if needed [\[ai.google\]]. If results aren't adequate, we can try a more powerful model like Gemini 2.5 Pro or GPT-4.1. If cost becomes the primary driver, we can benchmark against more efficient options like Gemini 2.0 Flash-Lite or GPT-4.1 nano later.

*(Transition: With a model chosen, let's start prompting and testing.)*

---

### 5. Iterate and validate against success criteria

1. **Draft Initial Prompt (incorporating Step 3):**
    * **Role:** `You are an expert Social Media Manager creating draft posts to promote blog content.`
    * **Context:** `Our brand voice is [describe: e.g., informative, friendly, slightly informal]. Our target audience is [describe].`
    * **Input:** `Here is the latest blog post: \n\n [Paste blog post text]`
    * **Instructions:** `Read the blog post. Identify the single most important takeaway. Generate two draft social media posts to promote this blog post:\n1. **Twitter/X:** Make it engaging, concise (under 280 chars). Include 2-3 relevant hashtags. \n2. **LinkedIn:** Make it professional and insightful, slightly longer than Twitter. Include 2-4 relevant professional hashtags. \n Ensure both drafts clearly link back to the core message of the post and include '[LINK]' as a placeholder for the blog post URL.`
2. **Run & Evaluate:** Generate drafts for an example blog post. Evaluate each draft (Twitter, LinkedIn) using the Step 2 scorecard.
    * *Result:* Might get 13/20. Perhaps the tone is off, or the hashtags are too generic (`#blog`, `#marketing`). Maybe the Twitter post is too long.
3. **Iterate on Prompt (Improvement 1):**
    * Use Markdown for structure.
    * Reinforce constraints: `**Twitter/X post MUST be under 280 characters.**`
    * Improve hashtag guidance: `Suggest *specific* hashtags relevant to the blog post's topic (e.g., if about AI, use #AI, #MachineLearning, not just #Tech).`
    * Specify tone more clearly: `Twitter tone: conversational and exciting. LinkedIn tone: professional and thought-provoking.`
4. **Run & Evaluate:** Scores improve (e.g., 16/20). Posts are better tailored.
5. **Iterate on Prompt (Improvement 2 - Few-Shot or Better Instructions):**
    * Add a good example *within* the prompt: `Example good Twitter post structure: [Catchy Hook]! Learn how [brief summary of key takeaway]. #RelevantTag1 #RelevantTag2 [LINK]`
    * OR: `For LinkedIn, start with a question or a key insight from the post to engage the audience.`
6. **Run & Evaluate:** Scores improve further (e.g., 18/20). Output is consistently better aligned with requirements.
7. **Consider Parameter Tuning:** If the tone is too bland, slightly increasing `temperature` might yield more creative (but potentially less predictable) drafts. Test carefully.
8. **Token Optimization:** Is the full blog post always needed? Perhaps providing only a summary or the first few paragraphs could work for simpler posts, reducing cost/latency. Test this trade-off.

*(Transition: Once we consistently get good drafts, let's see how it performs more broadly and compared to alternatives.)*

---

### 6. Benchmark and optimize

* **Benchmark:** Use the best prompt (from Step 5) with Gemini 1.5 Pro on 15-20 different blog posts covering various topics. Record scorecard results, generation time, and cost for each platform's draft.
* **Compare:** Run the *same* blog posts and prompt through alternative models. Record the same metrics (score, time, cost).
* **Analyze Results:** Compare average quality scores, consistency, speed, and cost. Model A might be best for LinkedIn (more nuance needed) while cheaper Model B is sufficient for Twitter drafts. Or one model might handle both well enough at an acceptable cost/speed.
* **Use an LLM as Automated Judge:** To get another perspective on quality, especially usefulness, use a separate LLM call.
  * **Setup:** Use a strong model with reasoning capabilities (e.g., Claude 3 Opus). Assign it the role of the *target stakeholder* (the busy Social Media Manager).
  * **Prompt Example:**
        ```prompt
        You are a busy Social Media Manager for our company. Your goal is to quickly post relevant updates based on our blog content to engage our audience on Twitter/X and LinkedIn. You value accuracy, engagement, platform suitability, and saving time.

        I will provide you with the original blog post summary and two social media drafts (one for Twitter/X, one for LinkedIn) generated by an AI assistant based on that summary.

        Please evaluate *only* the provided drafts based on the following criteria, thinking step-by-step:

        1. **Accuracy:** Do the drafts accurately reflect the key message of the blog summary? (Yes/No/Partially) Explain briefly.
        2. **Platform Fit (Twitter/X):** Is the Twitter/X draft concise, engaging, and suitable for that platform's audience? Does it use appropriate hashtags? (Scale 1-5, 5 being best) Explain.
        3. **Platform Fit (LinkedIn):** Is the LinkedIn draft professional, insightful, and suitable for that platform? Does it use appropriate hashtags? (Scale 1-5, 5 being best) Explain.
        4. **Engagement:** How likely are these drafts (as-is) to engage the target audience on each platform? (Scale 1-5) Explain.
        5. **Usefulness:** How much time would these drafts save you compared to writing from scratch? Would you use them as a starting point with minor edits, major edits, or not at all?

        **Blog Post Summary:**
        [Insert Blog Post Summary Here]

        **AI-Generated Drafts:**
        **Twitter/X:** [Insert Twitter Draft Here]
        **LinkedIn:** [Insert LinkedIn Draft Here]

        Provide your step-by-step evaluation based *only* on the criteria above.
        ```
  * **Run & Analyze Judge Output:** Run this evaluation prompt for the outputs generated by different models during benchmarking. The judge's structured feedback (especially on 'Usefulness' and the explanations) provides qualitative insights complementary to the scorecard metrics. This can help identify subtle issues or confirm if a draft truly meets the stakeholder's practical needs.
* **Decision & Validation:** Present the data (e.g., "Gemini 2.5 Pro averages 18/20 but costs $X per post; Claude 3.7 averages 16/20 but is 5x cheaper/faster") to the marketing team. They can decide based on the quality/cost/speed trade-off.

*(Transition: Before fully integrating this, even for internal drafts, think about potential pitfalls.)*

---

### 7. Ethical Considerations & Red Teaming

* **Bias:** Could the LLM adopt unintended biases from the blog post or its training data when generating social posts? (e.g., using non-inclusive language, making assumptions about the audience). Review generated drafts for subtle biases. Instruct the LLM to use neutral, inclusive language.
* **Harm:** A poorly worded or inaccurate social media post draft, if accidentally published without review, could spread misinformation or damage the brand's reputation. This *strongly* reinforces the need for the mandatory human review step. The AI is a draft assistant ONLY.
* **Privacy/Confidentiality:** If the blog posts contain sensitive pre-launch information, ensure the process (API calls, data handling) complies with company confidentiality policies.
* **Red Teaming:**
  * Input blog posts with potentially controversial or sensitive topics. Does the AI generate appropriate, neutral social media copy, or does it create something inflammatory or inappropriate?
  * Try feeding it ambiguous or internally contradictory source text. How does it handle the confusion?
  * Input text asking it to generate posts that violate platform guidelines (e.g., spammy content). Does it refuse or comply?
  * Test with very short or very long blog posts. Does it still extract meaningful points?
* **Mitigation:** Clearly define the AI's role as a *draft generator* requiring human oversight. Implement strict review processes. Add instructions to the prompt like `Avoid hype or making definitive claims not fully supported by the text.` and `If the topic is sensitive, adopt a neutral and factual tone.` Document the workflow and limitations.

---

This walkthrough shows how the mental model applies to generating social media drafts, emphasizing understanding requirements, defining success, iterative improvement, benchmarking, and crucial safety/ethical checks before deployment.
