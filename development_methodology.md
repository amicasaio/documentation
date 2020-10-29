# Development procceses

1. Analysis:
   The first phase of implementing a new functionality or bug fix is to make research.

   1. Study previous iteration to know what things went wrong and what wase done right.
   2. Find why this new functionality or bug fix needs to be implemented. What is it going to solve? Why we need to implement it?<br><br>

2. Planification:
   With data and info on table, we may now make decisions, plan and organize how we are going to implement said change.

   1. Establish which resources we need to design, architecth and program.
   2. Delegate who is going to make what task.
   3. Estimate times<br><br>

3. Design:
   Once we finish Planification, we may start designing and architeching a solution to solve the problem we are facing.<br><br>

4. TDD (Test Driven Development):
   In most of companies and Development teams, there is a separate department that is responsible of assuring that the code is going to work once deployed. This department is called **QA (quality assurance)**.<br><br>The Problem with having a different department that manages quality comes in several flavors:

   1. The depelopment and architecture team don't focus on quality and documentation of code on the initial fase of development. This will lead to more technical debt, more bug fixes on the future and less mantainable code.
   2. With having a different department responsible of code quality we enter into large iteration loops in which development team makes code and QA team tests code entering on a long feedback loop.<br><br>

   The solution proposed for this case is disolving QA department and implementing quality assurance onto the development team from the beggining, leading to quality focused code. From the first phase of programming, we first design and code tests that cover the needs of the functionality being implemented and the criteria defined on the beggining of the iteration.<br><br>

5. Evaluation, review & documentation
   Before we deploy our code. We take a last examination on our code verifying we favor verbose code over complex code (unless required otherwise). We document our code always taking in mind that we write code for 3 porpuses.

   1. Complete requirements.
   2. Other persons that are going to work with our code.
   3. Our future selves trying to understand our code.<br><br>

6. Deployment:
   When we finish everything we may deploy our code. Here it is important to have implemented a CI/CD (Continous integration/ continious development) workflow/pipeline to automate our deployments.<br><br>Now that everything is done, we have sufficient data to analyze to improve future iterations.