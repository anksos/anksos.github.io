---
title: "Babbling about AWS Solutions Architect Associate exam (SAA-C02)."
date: 2021-06-29
draft: false
toc: false
images:
tags:
  - anksos
  - aws
  - cloud
  - architect
  - solutions-architect
  - engineering
  - study-guide
---

So here we are, after a big pause since last year, finally got the inspiration to write something that might be helpful for others and will be a good reminder for me. Since this Covid-era many things have changed, our routines changed gradually, some kind of a roller coaster so writing left a little bit aside and to be honest some stuff that I wanted to write, was not able to share them.

That wasn't bad in the end because the stuff that I've been working with, helped me a lot in order to ace this exam and as you will read below it was not easy (or at least for me). Lately, I started having a very different approach on certifications, even though is still something that I believe someone at some point of time should achieve, I don't think that this is the ultimate goal.

I prefer to think certificates into milestones where you actually certifying your existing knowledge rather than a way to learn. If you think about, the first certificate that someone most probably achieves will be something from school, college or university, where you study many different topics and then after 1,2,3,X years you will have to have a summary of those via an exam process and this is how you will get your diploma. So vendor certifications don't believe are different.

Now, I know, out there is not utopia so let's move on.

Going to the exam and the study process, if someone was asking me to explain with one word the study process it would be **painful** and if they were asking me about the exam it self I would most probably say **intense** and don't worry I will explain the reason, just hold on.

There are mainly two ways to pass this exam (obviously this applies to other exams as well but let's not get into this rabbit hole). There is the hard way and the "easy" way and I am adding the word into quotes because even though is easier than the hard, still it needs some kind of spending time and is definitely time consuming. Now the biggest difference on those two ways from my point of view is the outcome, is the post-exam validation of your knowledge and understanding.

The first way and the toughest one is **hands-on + studying** and that might not be a **hard** requirement on the hands-on but it will help you a lot during the study process, as we said is the biggest pillar for the post-exam validation. The biggest focus here is your studying process going into deep to many different services using official documentation and resources.

The second way is **studying + practice exams** with a big focus on practice exams. This provides you certainly more "easy" way to go and pass the exam but not a well articulated understanding of what you have learned in depth. Also, as those exams are targeting into memorizing the Q&As certainly that's a short term memory in our brain so will not stay there forever. Often those practice exams are not always correct so careful about your source.

The way that I used and have been using always when I am going for an exam was the first (with a hybrid solution from the second). What I will explain is all the goods and bads that I did during this 4 weeks process and will provide with you all the learning material that I used and finally passed the exam.

So some weeks ago I received an invite to be included to an [AWS Academy Cloud Architecting course](https://aws.amazon.com/training/awsacademy/) (official AWS education) where I would have a month to finish the course and eventually get a voucher for taking the exam. This course provides you quite articulate information about the Solutions Architect mindset from AWS standpoint, hands-on experience as it has quite a lot of labs and a sandbox environment available to play with AWS services, in the end of the course it provides you with many, **but MANY** AWS Documentation resources, FAQs pages and more or less anything for you to read and go into deep in AWS Services and AWS Well-Architected Framework.

And I have to say... this education is awesome for giving you a very well understanding how you should think as an AWS Solutions Architect, working for AWS, so expect to see many scenarios on services and how you should think to solve the problem and provide a solution to a customer... but here was my pitfall.

After finishing the course and going through an extensive list of FAQs for services and AWS Documentation that were mentioned and recommended for the exam preparation I was kind of lost (you can find all the list of my study material on my [GitHub](https://github.com/anksos/aws-solutions-architect-associate)). Now you might be asking, why? The reason is simple, AWS is the best hyperscaler out there, they do stuff as noone else and their main thought is always **scale**. So you can imagine how extensive is the FAQ page for [Amazon EC2](https://aws.amazon.com/ec2/faqs/) or the documentation for [Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) and don't want to mention even Amazon S3 which is the service that is most well known and most probably is the first service that any customer out there is going to use.

So after finishing what AWS Academy course was recommending, I understood that this is not going to help me passing the exam, even though I went over and over through the [exam guide](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf) in order to find out where to focus my head was becoming more cloudy ( :D ). Btw something that was really helpful was the "[This is My Architecture](https://aws.amazon.com/architecture/this-is-my-architecture/?tma.sort-by=item.additionalFields.airDate&tma.sort-order=desc&awsf.category=categories%23databases%7Ccategories%23compute%7Ccategories%23storage&awsf.use-case=*all&awsf.industry=*all&awsf.language=*all&awsf.show=*all&awsf.format=*all)" page, if you have time go through it, has some architectures that are used during the exam.

Next step was to start searching on google other study guides (like the one I am writing right now) where people sharing their experience and help you go through the process. First two hits and boom, google gave me two results that became my best friend.

First one was a good reminder about VPCs and the way are working, with a funny [analogy having VPCs as City.](https://start.jcolemorrison.com/aws-vpc-core-concepts-analogy-guide/) Now, [J Cole Morris](https://start.jcolemorrison.com/)[on](https://start.jcolemorrison.com/) has other articles as well but the ones that I focused was the [IAM Policies in a Nutshell](https://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/) and the huge article about [AWS ECS](https://start.jcolemorrison.com/the-hitchhikers-guide-to-aws-ecs-and-docker/). If you want to deep dive on Amazon CloudFormation make sure to follow his [extensive guide](https://start.jcolemorrison.com/the-complete-cloudformation-guide/) about.

The other one was the big [learing path guide from Jayendra's blog](https://jayendrapatil.com/aws-certified-solutions-architect-associate-saa-c02-exam-learning-path/) many kudos to him articulating so many resources and being able to go in deep for each particular service.

I forgot to mention that during this time I went also through some AWS PartnerCast resources (those are resources available only for AWS Partners) and they were very helpful. Same for some AWS Youtube videos from Amazon, full list can be found in my [GitHub](https://github.com/anksos/aws-solutions-architect-associate) repository but here is a the list of the videos and AWS PartnerCast resources:

1. [Back to Basics: Bulk Data Storage](https://www.youtube.com/watch?v=hfqS3NPIApg)
2. [Back to Basics: Automating Virtual Machine Image Creation](https://www.youtube.com/watch?v=33di_iJ3b7w)
3. Exam Readiness: AWS Certified Solutions Architect - Associate (Digital training from PartnerCast)
    1. Module 1 - Design Resilient Architectures
    2. Module 2 - Design Performant Architectures
    3. Module 3 - Specify Secure Applications and Architectures
    4. Module 4 - Design Cost Optimized Architectures
4. The AWS Certification Quiz Show: Solutions Architect - Associate exam, Episode 6 (duration 30 minutes)
5. AWS PartnerCast Solutions Architect Associate Readiness - Practice Exam Questions (duration 1 hour)
6. [AWS Networking Fundamentals](https://www.youtube.com/watch?v=hiKPPy584Mg)  
    

Finishing all those someone would say, well you should be ready for going through the exam, it shouldn't be that hard, and this is true but working on daily basis on stuff with IBM Cloud and Azure mainly and studying in the evening about AWS, this wouldn't be that simple and what I like to do a couple of days before the exam is to get (most of the times) a practice exam that the vendor provides or an official resource (in general something that shouldn't be a dump) so I can understand what to expect from the real exam, if my time will be enough this helps me also to understand the topics that I need to focus prior the exam.

Exams are tricky, especially the ones that are very clever created. Exams are separated between lab oriented exams, multiple choice or scenario based exams. Even though lab based exams are definitely harder than the multiple choice and need real hands-on experience, are sometimes easier as the whole point is the outcome of a task (not all the time but majority of the times, Red Hat friends I understand your pain). Multiple choice exams from the other side if they have been made clever, (and the SAA-C02 is made) they providing you answers that are correct, but you will have to think as AWS wants you to think, that way in my honest opinion makes this exam harder than other exams I have taken in the same or even higher level.

And here was my second pitfall where actually passed the practice exam with 73% (72% is the pass). This showed me that even though I knew very well the services my study material was not preparing me for the exam but was preparing me for the post-exam validation and I will explain what I mean, hold on.

So what did I do a day before the exam, well, I found a person saying that this [education on Udemy by Stephane Maarek](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/) helped him to ace the exam and this is what I did, I took a day off before the exam, spent 12.5 hours in a row focus on the stuff that the AWS Practice exam pointed out and next day went extremely tired but satisfied with the outcome. The most important piece for me was going out of the exam room and be 100% sure that what I learned those 4 hard working weeks is something that for sure is not coming by memory but having deep understanding of AWS Services and most important Solutions Architecture. Stephane's material was created in a way that is purely focusing on the level that the services will be needed to known for the exam and nothing more. 100% focused material on the exam so you know exactly what to expect for each individual services.

I don't regret of course any of my pitfalls as I learned a lot through that process, overall was an awesome experience and helped me a lot to ace the exam.

Closing point, I fully understand that for some people might be looking as a long process and as I said there are easier ways to achieve the exam but in the end of the day, all that matters is being able to provide the best solutions for your Customers, if that's the way you can do it then is perfectly fine.

**EOF**
