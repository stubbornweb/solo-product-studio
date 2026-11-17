# Video: Solo agent vs subagents, 7 combinations for one coding task - Povilas Korop, AI Coding Daily

Taken on November 17, 2026. A 9-minute video by Povilas Korop (AI Coding Daily, Laravel Daily). He runs one coding task with a 7-phase plan in 7 setups: Opus, Sonnet and GPT-6.1-Sol alone, and lead models that hand each phase to Sonnet, Haiku or Luna helper agents. One run per setup. Video: https://www.youtube.com/watch?v=0MncEJDlva8. The text below is the YouTube auto-generated transcript, unchanged. It is a source for the agent factory idea.

0:00 Hello guys, a few days ago I posted on the channel the video comparing OPUS versus OPUS with Sonnet sub agents and that video exploded with your comments.
0:10 So at the moment there are more than 100 comments and I try to reply to all of them and read all of them but I have now
0:17 so many ideas for follow-ups. A lot of you basically are telling me that I'm holding it wrong so to speak with sub
0:24 agents and this is not the way to delegate. And I agree I need to dive deeper and I will. I have a lot of ideas for the follow-ups, but for now, I want
0:32 to finish that experiment with more testing, with more models. So, a lot of you in the comments said something like
0:39 this. You need to try OPUS with no sub agents and Sonnet with no sub aents. What would be the price and the quality?
0:46 Other people told me that I should use Haiku as a sub agent, which is much cheaper and now much better with Haiku
0:52 5.5. Also a few people mentioned Luna and SS so GPT models to maybe perform the same experiment and I did. So in
1:01 this video I will show you the result of seven different simulations with the same prompt. In three out of seven
1:08 options the agent went solo. So solo uset high or solo soul medium. And in
1:15 those cases, the prompt was implement all the actions in seven phases according to the plan and don't use any
1:22 sub agents. And the implementation plan looked like the so seven phases described in quite a detailed
1:29 specification phase by phase. So the main agent without sub agents knows basically what to do and can proceed
1:36 without me in the loop. And then other four cases with delegation to sub agents received this prompt. This is example of
1:44 ous with haiku complete all the phases using sub aents sequentially using model haiku medium effort and this was one of
1:53 your comments actually that sub agents should not have high effort because everything is thought of for them by
2:00 orchestrator at least it should work this way and also in this case to compare apples to apples I made the experiment exactly as in previous video
2:09 without any parallelization or other advice that you had in the comments.
2:14 Basically, we're testing the thing. If I just tell the orchestrator to delegate things to sub agents, is it more efficient, faster, cheaper, whatever?
2:22 Let's see the numbers. And side note, I also made experiment number eight in open code. This is for premium members.
2:29 So, I tried to delegate Opus 5.5 to Deep Seek as sub agents because you can configure Open Code to work exactly this way. So, the link will be in the
2:37 description below. This is for premium members. But here on YouTube, all the numbers from these seven experiments. So this is the main table that you probably
2:45 care about the most, the cost and the time. So in the previous video, two days ago, I showed the first two lines. So OP
2:52 was delegating to Sonnet was totally inefficient, more expensive, slower, and with roughly similar quality of the
2:59 code. But with other, your advice to delegate to Haiku was actually really good. So overall the API pricing was
3:07 cheaper than OPUS solo but it was twice as slow and I will talk about code quality and overall quality result in a
3:15 few minutes. For now let's compare the pricing. So sonnet solo was also pretty cheap and that was sonnet on high effort
3:23 level by the way. So this is probably the best economically viable option.
3:29 Just use sonnet instead of oos without sub aents. It will be fast and pretty cheap. Sonnet with haiku seems to be the
3:37 least expensive from ous from anthropic models but it was really much slower and we'll get to the quality in a minute.
3:47 Now with salt two experiments the time of delegating to Luna is astronomically slow because Luna well is slow and I
3:55 used Luna on high effort level and it was much more expensive than using just sole medium in this case and you can
4:03 compare the prices of GPT models versus anthropic models and you would think that Saul is much cheaper and it is if you use API pricing so this is nuanced.
4:13 I calculated the price of in this case GPT for example by tokens. So these tokens or I also reasked the agent
4:21 itself and then multiplied by API pricing because I didn't have the clear numbers of before and after in the usage
4:28 of subscription because I was running multiple things at the same time and I couldn't calculate that. So even on my LLM coding leaderboard, soul models seem
4:37 to be much cheaper than OPUS model, but the caveat is that its API pricing. In other videos, I compared the
4:45 subscription usage of Saul versus OPUS and it's roughly the same. So the level of how models are subsidized so to speak
4:54 on subscriptions is different for Anthropic and OpenAI. So if you use that on subscription, the price will be
5:01 probably roughly similar for Soul Solo and Oppo Solo on subscription usage. So the way to compare prices here is same
5:10 company versus other models of the same company. So for example, Soul Solo is ridiculously much cheaper than Soul with
5:18 Luna sub agents. But this is again I will repeat before applying a lot of your suggestions how to delegate and
5:25 orchestrate better. This is kind of a silly way to basically hey model let's use sub aents and do the magic but I
5:33 guess a lot of people would prompt exactly that without digging deeper into manual orchestration or what the sub
5:40 aents should do and how. So the goal here was to finish the exact same silly experiment with numbers. What are other
5:48 numbers? Let's go to code quality which is judged by automated tests passing and also static analysis and a few bugs that
5:57 were specifically tried to be reproduced by judge agent. So in two cases of the models even static analysis didn't pass
6:06 which was haiku. So in both cases of ous delegating to haiku and sonet delegating to haiku there were static analysis errors clear errors that were not fixed.
6:17 Then there's another column of asset building. So basically npm run build was it successful and it was successful for
6:24 all of them. And then another two columns where this specifically bad URL which was passed the test was passed
6:32 only by ous solo and set solo without sub aents. So in some cases it doesn't make sense to use sub aents because the
6:40 model itself is just better because it's working within the same context. And also one more bug was reproduced race
6:47 condition specifically in case of sonnet with haiku. So this is where I get back to the price of sonnet and haiku. It seems to be cheaper but for a reason.
6:58 Next I try to calculate and compare the amount of code written. Of course it's kind of a silly number but just to have
7:05 something quantifiable. So not sure if we can directly compare much here but two things that stand out. F soul with
7:13 solo wrote the least amount of code but still passing the main requirements of specification. So basically soul medium
7:22 was the most straightforward down to earth without too many ceremonies without extra tests. But are those tests
7:30 needed? Of course it depends on the scenario. So compare that to sonnet sub agent which had much more tests added.
7:37 The closest competitor is Sonnet versus Haiku, but I'm not sure was it forced by Sonnet as orchestrator or did Haiku add
7:45 many tests and those tests. So this is another slide. Some tests were indeed helpful. So fixed some errors for
7:53 example uset fixed something by set and haiku and there were a lot of details in pretty much all of those combinations.
8:01 So not sure if we can take away something from this slide. Probably the main takeaway is here. Sub agents sometimes bought useful evidence
8:10 sometimes. And this is kind of the final slide of that experiment. The main takeaway is start with capable solo
8:17 agent because solo agents performed really well in this experiment. In most cases better, sometimes much better,
8:25 much cheaper than sub agent implementation. But again, of course, this was silly sub agent implementation.
8:33 And this is what the slide means by measurable benefit. So the workers sub agents should be added with much more
8:42 deeper thoughts into what they do, how they do it, how the context is passed, how the results are verified and stuff
8:50 like that. And this is what I will dive deeper into in the future videos. My head is kind of exploding with ideas
8:57 from your comments. So stuff like parallel sub aents, input caching, specifying the sub aents with the roles
9:04 separately, reviewing of the code between for example cloud code and codecs in the meantime, debugging the context and stuff like that. I will experiment with a lot of those things.
9:14 So subscribe to the channel to not miss any of those videos. And what do you think about this experiment? Let's discuss as usual in the comments below.
9:22 That's it for this time and see you guys in other
