---
date: "2021-02-26T00:00:00Z"
subtitle: My favourite idea of GEB
tags:
- GEB
title: Stepping outside the System
---

In the book GEB, there is puzzle called MU puzzle, which goes this way...

![MU Puzzle from Wikipedia](https://i.imgur.com/paaaPwj.png) 

After trying rigorously for half an hour, I stopped and suspected if it is possible to solve this. After some googling, I found that my suspicion was right, it is not possible to obtain the string MU from MI, with the given rules of production. The reason is hard to find. But below is a simple explanation. 

One way to think of it is, you have "x" number of I s(read as eyes). And at the end you want x to be equal to zero. The second and third rules change the number of I s. As per the second rule number of I s becomes 2x, while the third rule changes it to x-3. The desired outcome x being zero, implies that x has to be divisible by 3(because zero is divisible by 3). Initially, if x is not divisible by 3, then

1. 2x will also not be divisible by 3. Doubling the number will not make it divisible by 3, if it isn't already divisible by 3
2. x - 3 will also not be divisible by 3. Remove 3 from the number will not make it divisible by 3, if it isn't already divisible by 3

As per the puzzle, we start with x = 1, which is not divisible by 3; that implies we cannot ever reach a state where x is divisible by 3; hence you cannot ever get x = 0, which is desired. To conclude, we cannot achieve the state MU by any combination of the rules provided.

This is not an easy thing for a person to come up. But here is another simple example. If given `MI`, can you produce the string `I`, using the given rules. No! Because there is no rule, which reduces the count of M s.

But when you feed the same problem to a machine - feeding the rules and and axiomatic string MI, it will never stop, it will continue to form new strings and check if they are MU or not.

![Taken from GEB Pg. 40](https://imgur.com/SzXUzZg.png)

The machine does not stop on its own, because it cannot think outside of the rules. We as a human, when rules of a system are given, are capable of stepping out of the system and reflect on the rules. Doug argues that would be the main characteristic of a truly intelligent machine, being able to step out of the rules, reflect on them and be capable of modifying the instructions based on the outcome.

```
It is inherent property of intelligence that it can jump out of the task which it is performing and survey what it has done; it is always looking for and often finding, patterns.

Pg.37 GEB
```



- DH asks u to solve MU puzzle
- Plot twist, later u realize u are unable to get it.
- A machine when given rules of product would run endlessly. But we humans have the ability to step outside of system and think about the rules than just follow the rules blindly.
- AI implications machine being able to observe its own actions and modify as per the result.

## Philosophical Implications

What excites me more is this philosophical implication of the idea. There is a line in GEB..

```
Of course, there are cases where only a rare individual will have the vision to perceive
a system which governs many people’ lives, a system which had never before even been
recognized as a system; then such people often devote their lives to convincing other
people that the system really is there, and that it ought to be exited from!
 
Pg.37 GEB
```

And I found its explanation in the [MIT OCW notes]([Chapter 5: The MU–Puzzle (mit.edu)](https://ocw.mit.edu/high-school/humanities-and-social-sciences/godel-escher-bach/lecture-notes/MITHFH_geb_v3_5.pdf))

![People who have stepped outside the system](https://imgur.com/6kD9T4L.png)

### My interpretation 

It means one as an individual not just has to follow rules given to him/her(in a community or society or workplace), he/she must also "step outside the system" and reflect upon the rules - question the rules - "is it really true?", "what are its outcomes?", "why does one have to do this way?". This I believe helps an individual to question conformist behavior  and help him/her become an independent thinker. Being able to think on own is a rare quality!



## Footnotes

- While many condemn Karl Marx due to what it has caused in USSR and China. A proper look into the history will reveal that Karl Marx was largely misinterpreted by the cruel leaders. A good explanation regarding it can be found in watch Episode 1 of [Genius of the Modern World | Netflix](https://www.netflix.com/in/title/80186252). You can also read the 6th point in the article - [BBC Arts - BBC Arts - Forward thinkers: How Marx, Nietzsche and Freud shaped the lives of millions](https://www.bbc.co.uk/programmes/articles/4CVpPWQkwbDzt4w2RjNHf2S/forward-thinkers-how-marx-nietzsche-and-freud-shaped-the-lives-of-millions)

- Image of the puzzle taken from [MU puzzle - Wikipedia](https://en.wikipedia.org/wiki/MU_puzzle)
- Image of tree taken from Page number 40 of GEB
