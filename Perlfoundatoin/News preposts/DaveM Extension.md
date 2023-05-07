
# DaveM Extension report

Perl is a powerful programming tool that is deeply embedded in Linux and continues to form a critical component in diverse sets of applications.  As such it needs continuous maintenance, optimisation and bug fixing. A mature language still relies of magic hidden away in its core that handles things like memory allocation, reference counting, and cope with complex algorithms which the initial developers may not have considered when the interpreter was being developed. 

Two key strengths of Perl are 1) that it allows more than one way to do things 2) it remains backwardly compatible; new evolutions must not break old things. Inevtitably this philosphy has allowed developers to stress the language to the extent that bugs and failures appear, and this that relies on continued maintainance of the core.  

Dave Mitchell is one of our core deleopers who has taken on some of the repsponisibility of maintenance.  While initially it has been largely to look at performance and bug fixes.  His current grant support is  looking to be renewed/extended, and it is important to look at a few key features of his work so far.

### Deparsing and Test -deparse

Deparsing is a way that Perl can generate Perl code form the compiled structure generating by the Perl parser.   [This](https://www.perl.com/article/89/2014/5/15/Debunk-Perl-s-magic-with-B-Deparse/) is a useful summary of deparse.  Brian DeFoy has aso gives useful [insight](https://www.effectiveperlprogramming.com/2010/05/use-bdeparse-to-see-what-perl-thinks-the-code-is/) on how Deparse can de-obfuscate Perl code, pick up errors etc.  This has taken  around 30 hours of work.  It certainly highlights a feature I knew very little about at all. 

### Rework

He has done a complete reimplementation of scope internals: by scope we mean blocks of code  which relate to a particular context.  The context may represent the program as a whole, a subroutine, or a loop.  There is reduction of reliance on recursion, the internal code is made more readable, and steps to prevent buffer overrun, improve sprintf, list assiugnment etc.

### Stand up and be counted

While fixing specific issues that have been raised is important, there are key components that may be repsonsible for multiple failures and many issues. These are difficult to spot. One of these relates to the way Perl manages references.  This [post by David Farrell](https://www.perl.com/article/the-trouble-with-reference-counting/#:~:text=Perl%20uses%20a%20simple%20form,belong%20to%20the%20block%20scope.) highlights how complex this problem is.  When variables go out of scope and no longer required they are (or should be) cleaned up, and the memory used freed.  If this space are not correctly freed, then there is poetntial for memory leakage and the routines taking up more and more space through each call or conversely innapropriate removal of references mean that a variable that is expected to be accessed can not be.  Both lead to failures and are potentially responsible for many “unfixable errors” in Perl modules.  But reference counting is not simple.   Many objects nest references deeply, and further more, Perl owes some of its flexibility to allowing programming behaviours that may be frowned on in other languages.  The price of this flexibility is a rather large amount of stress on Perl core.  

So to identify these problems Dave has created a wrapper for over 250 functions.  This has led to a feature that allows references to be counted, to allow identifications of points of failure.  This wrapper can be optionally activated with a minor amount of slowdown, but once the robustness of the code has been verified, can be removed.  This work is enormous, and has taken close to 200 hours of work.  This extra work has thus eaten up a lot of the grant very early in the term.  A rather big risk which, Dave himself admits, may not be completely successful.  At the same time, this one mega-effort has the potential to transform the robustness of Perl core.


The work that our Core developers do including Dave Mitchell, Tony Cook and many others do are what makes Perl continue to be relevant.  Undoubtedly this is a task that reuires a depth of knowledge that is not ubiquitous in the community that depends on Perl.

