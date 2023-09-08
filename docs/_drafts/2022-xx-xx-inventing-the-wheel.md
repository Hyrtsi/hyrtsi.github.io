

Inventing the wheel again

Imagine you would start programming a time and date utility library in C++. Why c++? Don't worry about that.

Maybe for an app that looks like Google Calendar. Your boss tells you to do this and tells that you have two weeks to finish it. Doesn't sound too bad?

Now, what if I told you that there are already thousands of codebases, github repos, libraries, closed-ware code that have implemented the same functionality that you're about to write?

Today I want to discuss code reuse, inventing the wheel again, open source and more.

# Programmer literacy

Imagine a good doctor. They know the best tools for their craft whether it's surgical tools, best medicine for given symptoms or the best way to fix a broken bone. They have to read a lot of literature and journals. They attend to seminars regularly and discuss with their colleagues.

Programmers need similar practises. They have to read other people's code. Preferably both inside and outside their own organization. They have to learn all the necessary features and best practises in the programming languages, frameworks and libraries they use.

Before we start writing code we must understand the problem. After that we need to think if there is a problem to begin with. Does a single line of code need to be written for the problem? That is the lazy and smart approach to problems.

Then we need to understand if the problem has already been solved. Probably the answer is yes. But there are a lot of unsuitable solutions out there. Maybe for a simple task you need to include a huge monolithic library like Boost. That isn't acceptable for all projects. Or the license of the library isn't to your liking. Maybe the programming language is wrong. 

Even if you don't resort into using somebody elses code "directly" you can get inspired by that code. Maybe you'll use their testcases. Or copy their architecture. Or you don't use their antipatterns. Code shares information in so many ways. It's not only discussion between the developer and the compiler. It's also discussion between developers. It's instructions of the problem. It's a description how one or several persons have faced and solved some problem.

