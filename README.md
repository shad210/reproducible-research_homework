# Reproducible research: version control and R

**Question 4:**

a. The two graphs which are produced from the random walks show continuous movement across the x and y axes, over time. The two graphs are noticably different in both pathway and scale.

b. A random seed specifically defines a sequence of randomly generated numbers so that it is reproducible. In other words, the 'random seed' which is assigned to this sequence can be used by any other user to output the same pattern of numbers regardless of working environment. Thus, truly random data can still be generated, but multiple different researchers can do analysis independently of each other on the same random data set.

Random seed code change image:

![image](https://github.com/shad210/reproducible-research_homework/assets/150149671/49bc9ea6-d92c-410a-81ce-d2ed5b204a3b)

------------------------------------------

**Question 5**

a. Using the nrow and ncol functions in R, the dataframe has 33 rows, and 13 columns.

b. When visualisating the data in ggplot, it is clear that both the genome size and the virion volume need to be logarithmically transformed, so a log transformation is what is needed to fit the data for linear modelling.

c. By creating a linear model for the log transformed data, and then viewing a summary of the model, we generate the table below:

![image](https://github.com/shad210/reproducible-research_homework/assets/150149671/4a5b88ad-dd46-4699-ae5a-43637714f165)

The log of the scaling factor is the estimate for the intercept of 7.0748. To find the actual scaling factor, we need to exponentiate the value by using e^(7.0748) which gives us a value of 1181.8.
The exponent factor is simply the log_genome_length estimate value of 1.5152 - here there is no need to exponentiate since this factor is already an exponent. The p-value for α is 2.28e-10, and the p-value for β is 6.44e-10, meaning they are both statistically significant.

So, α = 1.5152, and β = 1181.8

The exponent factor α is very similar to the factor found in the paper (1.5152 in our model vs. 1.52 in the paper) and lies within the 95% CI for the factor, and the scaling factor is also very similar to the factor found in the paper (1181.8 in our model vs. 1182 in the paper).

d. The code to reproduce the figure is as follows:

```{r}
#Read the csv from the paper, and define as a dataframe
Cui_data <- read.csv("Cui_etal2014.csv")

#We need to log transform both the genome length and the virion volume
log_genome_length <- log(Cui_data$Genome.length..kb.)
log_virion_volume <- log(Cui_data$Virion.volume..nm.nm.nm.)

#Create our plot with the linear model added using geom_smooth
ggplot(aes(x = log_genome_length,y = log_virion_volume), data = Cui_data) +
  geom_point() +
  geom_smooth(method = "lm", formula = y ~ x, se = TRUE, color = "blue") +
  xlab("log[Genome length (kb)]") +
  ylab("log[Virion volume (nm^3)]") +
  theme_bw()

```
Which then outputs the following figure:

![image](https://github.com/shad210/reproducible-research_homework/assets/150149671/62961d52-c864-4331-ab0a-6c6a9aa89a18)

e. To estimate the volume of a 300 kb dsDNA virus, we must use our allometric model V(L) = β(L^α). Thus V(300) = 1181.8(300^1.5152) = 6697000 nm^3

---------------------

Bonus:



## Instructions

The homework for this Computer skills practical is divided into 5 questions for a total of 100 points (plus an optional bonus question worth 10 extra points). First, fork this repo and make sure your fork is made **Public** for marking. Answers should be added to the # INSERT ANSWERS HERE # section above in the **README.md** file of your forked repository.

Questions 1, 2 and 3 should be answered in the **README.md** file of the `logistic_growth` repo that you forked during the practical. To answer those questions here, simply include a link to your logistic_growth repo.

**Submission**: Please submit a single **PDF** file with your candidate number (and no other identifying information), and a link to your fork of the `reproducible-research_homework` repo with the completed answers. All answers should be on the `main` branch.

## Assignment questions 

1) (**10 points**) Annotate the **README.md** file in your `logistic_growth` repo with more detailed information about the analysis. Add a section on the results and include the estimates for $N_0$, $r$ and $K$ (mention which *.csv file you used).
   
2) (**10 points**) Use your estimates of $N_0$ and $r$ to calculate the population size at $t$ = 4980 min, assuming that the population grows exponentially. How does it compare to the population size predicted under logistic growth? 

3) (**20 points**) Add an R script to your repository that makes a graph comparing the exponential and logistic growth curves (using the same parameter estimates you found). Upload this graph to your repo and include it in the **README.md** file so it can be viewed in the repo homepage.
   
4) (**30 points**) Sometimes we are interested in modelling a process that involves randomness. A good example is Brownian motion. We will explore how to simulate a random process in a way that it is reproducible:

   - A script for simulating a random_walk is provided in the `question-4-code` folder of this repo. Execute the code to produce the paths of two random walks. What do you observe? (10 points)
   - Investigate the term **random seeds**. What is a random seed and how does it work? (5 points)
   - Edit the script to make a reproducible simulation of Brownian motion. Commit the file and push it to your forked `reproducible-research_homework` repo. (10 points)
   - Go to your commit history and click on the latest commit. Show the edit you made to the code in the comparison view (add this image to the **README.md** of the fork). (5 points)

5) (**30 points**) In 2014, Cui, Schlub and Holmes published an article in the *Journal of Virology* (doi: https://doi.org/10.1128/jvi.00362-14) showing that the size of viral particles, more specifically their volume, could be predicted from their genome size (length). They found that this relationship can be modelled using an allometric equation of the form **$`V = \beta L^{\alpha}`$**, where $`V`$ is the virion volume in nm<sup>3</sup> and $`L`$ is the genome length in nucleotides.

   - Import the data for double-stranded DNA (dsDNA) viruses taken from the Supplementary Materials of the original paper into Posit Cloud (the csv file is in the `question-5-data` folder). How many rows and columns does the table have? (3 points)
   - What transformation can you use to fit a linear model to the data? Apply the transformation. (3 points)
   - Find the exponent ($\alpha$) and scaling factor ($\beta$) of the allometric law for dsDNA viruses and write the p-values from the model you obtained, are they statistically significant? Compare the values you found to those shown in **Table 2** of the paper, did you find the same values? (10 points)
   - Write the code to reproduce the figure shown below. (10 points)

  <p align="center">
     <img src="https://github.com/josegabrielnb/reproducible-research_homework/blob/main/question-5-data/allometric_scaling.png" width="600" height="500">
  </p>

  - What is the estimated volume of a 300 kb dsDNA virus? (4 points)

**Bonus** (**10 points**) Explain the difference between reproducibility and replicability in scientific research. How can git and GitHub be used to enhance the reproducibility and replicability of your work? what limitations do they have? (e.g. check the platform [protocols.io](https://www.protocols.io/)).
