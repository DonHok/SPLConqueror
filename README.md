[![Build status](https://travis-ci.org/se-passau/SPLConqueror.svg?branch=master "Build status")](https://travis-ci.org/se-passau/SPLConqueror)

# SPL Conqueror Project Structure

<details>
<summary>
SPL Conqueror is a library to identify and visualize the influence of configuration options on non-functional properties such as performance of footprint of configurable software systems.
It consists of a set of sub-projects, which we roughly explain in the following. For further details, we refer to the dedicated sections.
</summary>

* The core project ("**SPLConqueror_Core**") provides basic functionalities to model the variability of a software system including their configuration options and constraints among them.

* The **MachineLearning** sub-project provides an algorithm to learn a performance-influence model describing the influence of configuration options on non-functional properties. It also specifies interfaces for satisfiability checking of configurations with respect to the variability model and optimization with respect to finding an optimal configuration for a given objective function and non-functional property.

* The **PyML** sub-project provides a set of interface to [*scikit* learn](http://scikit-learn.org/stable/), which is a machine-learning framework implemented in python. Using this interface different regression techniques of *scikit* learn can be used.

* The **CommandLine** sub-project offers an interface to automatically execute experiments using different sampling strategies and different machine-learning techniques.
To specify the experiments, SPL Conqueror offers a set of commands, which we explain in the dedicated [section](#commandline). 

* The **PerformancePrediction_GUI** provides a graphical user interface to learn performance-influence models based on a learning set of configurations. Using this GUI, specific sampling strategies can be used.

* The **SPLConqueror_GUI** provides a set of different visualisations that can be used to further understand a learned performance-influence model. 

* The **ScriptGenerator** provides an interface to generate script files that can be used in the CommandLine sub-project.

* The **VariabilityModel_GUI** offers the possibility of defining a variability model of the configurable system being considered.

* The **Persistence** sub-project offers the possibility of writing objects to the storage device. 
It can be used to continue the execution of script files that are aborted in their execution.

</details>



## How to install SPL Conqueror

<details>
<summary>
On Ubuntu 16.04
</summary>

1. Clone git repository

2. Install Mono and MonoDevelop
```
sudo apt install mono-complete monodevelop
```

3. Start MonoDevelop and open the root project:
```
<SPLConquerer-GitRoot>/SPLConqueror/SPLConqueror.sln
```

4. Perform a right-click on every project of the solution and select the preferred target framework (e.g., .NET4.5) in **Options -> Build -> General**

5. Perform a right-click on the solution and select **Restore NuGet packages**
Be aware that an internet connection is required to perform this step.

6. Build the root project
</details>

<details>
<summary>
On a Mac (OS X (10.11.6))
</summary>

a1. Clone git repository

2. Download and install latest Xamarin-IDE from https://www.xamarin.com

3. Start Xamarin-IDE and open the root project:
```
<SPLConquerer-GitRoot>/SPLConqueror/SPLConqueror.sln
```

<!-- 5. mkdir "<SPLConquerer-GitRoot>/SPLConqueror/packages"

6. cd "<SPLConquerer-GitRoot>/SPLConqueror/packages"

7. Install dependencies:

  * nuget install Accord -version 2.12.0.0
  * nuget install Accord.Math -version 2.12.0.0
  * nuget install AForge -version 2.2.5
  * nuget install AForge.Math -version 2.2.5
  * nuget install ILNumerics -version 3.3.3.0
  -->

-->

5. Build root project
</details>

<details>
<summary>
On Windows 10
</summary>

1. Clone git repository

2. Install Visual Studio

3. Open Visual Studio and open the solution

4. Perform a right-click on the solution and select **Restore NuGet Packages**

<!--
4. Perform a right-click on the following projects and add the following NuGet packages by selecting "Manage NuGet packages...":
  * MachineLearning:
      * Accord
      * Accord.Math
  * SPLConqueror_GUI:
      * ILNumerics
      * ILNumerics.Native
  * SolverFoundationWrapper:
      * Microsoft.Solver.Foundation
-->
    
5. Build the root project
</details>

<details>
<summary>
Troubleshooting
</summary>
1. NuGet

If the NuGet is not able to restore the packages, the following packages have to be added to the projects:
  * MachineLearning:
	  * Accord
	  * Accord.Math
  * SPLConqueror_GUI:
	  * ILNumerics (v3.3.3.0)
  * SolverFoundationWrapper:
	  * Microsoft.Solver.Foundation (>= v3.0.0)
	
Additionally, if the package *Microsoft.Solver.Foundation* is needed, the following steps should be performed:
1. Create a directory for the dll:
	```
	mkdir "<SPLConquerer-GitRoot>/SPLConqueror/dll"
	```

2. Copy *Microsoft.Solver.Foundation.dll* (>= v3.0.0) to "<SPLConquerer-GitRoot>/SPLConqueror/dll"
</details>

## How to use SPLConqueror

### GUI

SPL Conqueror provides four different graphical user interfaces.

<details>
<summary>VariabilityModel_GUI</summary>

The VariabilityModel_GUI can be used to define the variability model of a configurable system or to modify existing models. To create a new variability model for a system, fist use *File>New Model*. Then, an empty model containing only a root configuration option is created. New options can be added to the model by a right click on an existing option that should be the parent option of the new one. In the *Create new Feature* dialogue, it is possible to define whether the new option is a binary or a numeric one. For numeric options, also a minimal and maximal value of the value domain have to defined. Besides, if only a subset of all values between the minimal and the maximal value of the domain are allowed, a specific step function can be defined. In this function it is possible to use an alias for the numeric option (*n*). In the following, we give two examples of the step functions: 

  * n + 2 (using this function, only even or odd values depending on the minimal value of the value domain are allowed)
  * n * 2 (using this function, the minimal value is multiplied by two until the maximal value is reached)

Additionally, constraints between different configuration options can be defined using *Edit>Edit Constraints*. Last, an alternative group of options can be created using *Edit>Edit Alternative Groups*.

An example for a variability model is given below: 
```
<vm name="exampleVM">
  <binaryOptions>
    <configurationOption>
      <name>xorOption1</name>
      <outputString/>
      <prefix/>
      <postfix/>
      <parent/>
      <children/>
      <impliedOptions/>
      <excludedOptions>
        <option>xorOption2<option>
      </excludedOptions>
      <defaultValue>Selected</defaultValue>
      <optional>False</optional>
    </configurationOption>
    <configurationOption>
      <name>xorOption2</name>
      <outputString/>
      <prefix/>
      <postfix/>
      <parent/>
      <children/>
      <impliedOptions/>
      <excludedOptions>
        <option>xorOption1<option>
      </excludedOptions>
      <defaultValue>Selected</defaultValue>
      <optional>False</optional>
    </configurationOption>
  </binaryOptions>
  <numericOptions>
    <configurationOption>
      <name>numericExample</name>
      <outputString/>
      <prefix/>
      <postfix/>
      <parent/>
      <children/>
      <impliedOptions/>
      <minValue>1</minValue>
      <maxValue>10</maxValue>
      <stepFunction>numericExample + 2</stepFunction>
      <defaultValue>10</defaultValue>
    </configurationOption>
  </numericOptions>
</vm>
```
</details>

<details>
<summary>PerformancePrediction_GUI</summary>

The PerformancePrediction_GUI provides an interface to learn performance-influence models. 
To use this GUI, first a variability model and dedicated measurements of the system has to be provided.
Afterwards, in the middle are of the GUI, a binary and numeric sampling strategies has to be selected to define a set of configuration used in the learning process. 
To customize the machine-learning algorithm all of its parameters can be modified. 
To start the learning process, press the *Start learning* button.

*Note:* Please make sure that bagging will be set to false when using this GUI. If bagging is selected, a set of models are learned and all of them are presented in the GUI, which makes understanding the model hard.

After the learning is started, the models, which are learned in an iterative manner are displayed in the lower part of the GUI. 
Here, the model is split by the different terms, where each term described the identified influence of an individual option or an interaction between options. 
</details>

<details>
<summary>SPLConqueror_GUI</summary>

This GUI can be used to visualize a learned performance-influence model. 
</details>

<details>
<summary>Script generator</summary>

The Script generator can be used to define .a-script files that are needed in the CommandLine project. 
</details>

### CommandLine

The CommandLine sub-project provides the possibility to automatically execute experiments using different sampling strategies on different case study systems.
To this end, a .a-script file has to be defined. 
In the following, we explain the different commands in detail. 

<details>
<summary>Basic command-line commands</summary>

As SPL Conqueror provides a lot of commands, some of which are vital for an execution of SPL Conqueror.
Unless the GUI is not used, knowing the basic command-line commands is crucial for the user.

##### Log command

```
log <path_to_a_target_file>
```

Using this command, the output of SPL Conqueror is redirected to the given file.
SPL Conqueror will automatically create this file if it does not existis, otherwise the file will be overwritten. Additionally, an .log_error file is created, which includes the errors during the execution.
*Note*: If the ```log```-command is missing, the output will be prompted directly to the console.

For example:
```
log C:\exampleLog.log
```
or 
```
log /home/username/exampleLog.log
````


##### Loading the variability model

```
vm <path_to_model.xml> 
```

To actually perform experiments on a given system, a variability model that covers the variability domain of the system being considered has to be defined. 
This can be done using the **VariabilityModel_GUI**.

For example: 
```
vm C:\exampleModel.xml
```
or 
```
vm /home/username/exampleModel.xml
```

Such a variability model generally consists of binary and numeric options, with their properties, and optionally boolean and nonBoolean constraints between configuration options and has to be in a .xml-file.

For instance, a variability model with the name exampleVM is defined as follows:
```
<vm name="exampleVM">
  <binaryOptions>
    <configurationOption>
      <name>xorOption1</name>
      <outputString/>
      <prefix/>
      <postfix/>
      <parent/>
      <children/>
      <impliedOptions/>
      <excludedOptions>
        <option>xorOption2<option>
      </excludedOptions>
      <defaultValue>Selected</defaultValue>
      <optional>False</optional>
    </configurationOption>
    <configurationOption>
      <name>xorOption2</name>
      <outputString/>
      <prefix/>
      <postfix/>
      <parent/>
      <children/>
      <impliedOptions/>
      <excludedOptions>
        <option>xorOption1<option>
      </excludedOptions>
      <defaultValue>Selected</defaultValue>
      <optional>False</optional>
    </configurationOption>
  </binaryOptions>
  <numericOptions>
    <configurationOption>
      <name>numericExample</name>
      <outputString/>
      <prefix/>
      <postfix/>
      <parent/>
      <children/>
      <impliedOptions/>
      <minValue>1</minValue>
      <maxValue>10</maxValue>
      <stepFunction>numericExample + 1</stepFunction>
      <defaultValue>10</defaultValue>
    </configurationOption>
  </numericOptions>
</vm>
```

The nodes *outputString*, *prefix* and *postfix* can be ignored for now. The *parent*-node can either be empty or have an *option*-node as child with the name of the option, that is the parent of the current option(similar to excludedOption). The *children*, *impliedOptions* and *excludedOptions*-nodes are analogous with the exception that they can contain several options and define the children and implied options of the current option and the options that are excluded by this option if it is selected. *stepFunction* defines the function that decides which values the numeric option can have. For further real world examples we refer to [Suplemental Material](http://www.infosun.fim.uni-passau.de/se/projects/splconqueror/#supMat).

##### Loading the measurements

```all <path_to_a_measurement_file>```

This command defines the file containing all measurements of a given system. 
Exampls for this command are: 
```
all C:\exampleMeasurements.xml
```
or 
```
all /home/username/exampleMeasurements.xml
```

For this kind of files, two different formats are supported. 
The first one is a .csv format. 
Here each line of the file contains one the measurements for one configuration of the system.
This file should contain a header that defines the names of the configuration options as well as the non-functional properies being considered.
The second format is a .xml format. 
A short example using this format is provided in the following: 

```
<results>
  <row>
    <data column="Configuration">xorOption1,</data>
    <data column="Variable Features">numericExample;1</data>
    <data column="nfp1">1234</data>
    <data column="nfp2">2345</data>
  </row>
  <row>
    <data column="Configuration">xorOption2,</data>
    <data column="Variable Features">numericExample;10</data>
    <data column="nfp1">4321</data>
    <data column="nfp2">5432</data>
  </row>
</results>
```

Optionally you, in case you have knowlegde about the relative deviation, you can also provide the deviation values in a coma separated format. The highest rel. deviation value is used as metric of how accurate the learning can and therefore used as the abort error for the learning.
```
<results deviation="2;3;9.13">
  .
  .
  .
</results>
```


Further real world examples of measurements in xml format are provided in the [Suplemental Material](http://www.infosun.fim.uni-passau.de/se/projects/splconqueror/#supMat).

Alternatively, the measurements can be provided in a *csv*-format. Thereby, the first row has to be a header with the name of the binary and numeric options and the names of the non functional properties. In the column of binary options there has to be either true or false, indicating whether the feature was selected in this configuration or not, and in the columns of numeric options the values that were selected in this configuration. In the columns are the values of the properties that were measured for this property. So if we format the above example in csv format: 

| xorOption1; | xorOption2; | numericExample; |  nfp1; |  nfp2; |
| :---------: | :---------: | :-------------: | :----: | :----: |
| true;       | false;      | 1;              | 1234;  | 2345;  |
| false;      | true;       | 10;             | 4321;  | 5432;  |

*Note*: The element separator is ```;```, whereas the line separator is ```\n```.

#### Loading machine-learning settings

Before starting the learning process upon the loaded data, one can adjust the settings used for machine learning. SPL Conqueror supports multiple different settings to refine the learning. A list of all currently supported settings is presented in the following:

| Name  | Description | Default Value | Value Range |
| :---: | :---------: | :-----------: | :---------: |
| lossFunction | The loss function on which bases options and interactions are added to the influence model | RELATIVE | RELATIVE, LEASTSQUARES, ABSOLUTE |
| epsilon | The epsilon within the error of the loss Function will be 0. A epsilon of 0 is equal to this feature not being present | 0 | int |
| parallelization | Turns the parallel execution of model candidates on/off. | true | true, false |
| bagging | Turns the bagging functionality (ensemble learning) on. This functionality relies on parallelization (may require a larger amount of memory). | false | true, false |
| baggingNumbers | Specifies how often an influence model is learned based on a subset of the measurement data. | 100 | int |
| baggingTestDataFraction | Specifies the percentage of data taken from the test set to be used in one learning run. | 50 | int |
| useBackward | Terms existing in the model can be removed during the learning procedure if removal leads to a better model. | 50 | int |
| abortError | The threshold at which the learning process stops.(abortError can also be set via measurement file, see measurement section for more information) | 1 | double |
| limitFeatureSize | Terms considered during the learning procedure can not become arbitrary complex. | false | true, false |
| featureSizeThreshold | The maximal number of options participating in one interaction. | 4 | int |
| quadraticFunctionSupport | The learner can learn quadratic functions of one numeric option, without learning the linear function apriory, if this property is true. | true | true, false |
| crossValidation | Cross validation is used during learning process if this property is true. | false | true, false |
| learn-logFunction (alternatively: learn_logFunction) | If true, the learn algorithm can learn logarithmic functions such as log(soption1). | false | true, false |
| learn-accumulatedLogFunction (alternatively: learn-accumulatedLogFunction) | Allows the creation of logarithmic functions with multiple features such as log(soption1 * soption2). | false | true, false |
| learn-asymFunction (alternatively: learn_asymFunction) | Allows the creation of functions with the form 1/soptions. | false | true, false |
| learn-ratioFunction (alternatively: learn_ratioFunction) | Allows the creation of functions with the form soptions1/soptions2. | false | true, false |
| learn-mirrowedFunction (alternatively: learn_mirrowedFunction) | Allows the creation of functions with the form (numericOption.maxValue - soptions). | false | true, false |
| numberOfRounds | Defines the number of rounds the learning process have to be performed. | 70 | int |
| backwardErrorDelta | Defines the maximum increase of the error when removing a feature from the model. | 1 | double |
|minImprovementPerRound | Defines the minimum error in improved a round must reach before either the learning is aborted or the hierarchy is increased for hierarchy learning. | 0.1 | double |
| withHierarchy | Defines whether we learn our model in hierarchical steps. | true | true, false |
| bruteForceCandidates | Defines how candidate features are generated. | false | true, false |
| ignoreBadFeatures | Enables an optimization: we do not want to consider candidates in the next X rounds that showed no or only a slight improvement in accuracy relative to all other candidates. | false | true, false |
| stopOnLongRound | If true, stop learning if the whole process is running longer than 1 hour and the current round runs longer then 30 minutes. | true | true, false |
| candidateSizePenalty | If true, the candidate score (which is an average reduction of the prediction error the candidate induces) is made dependent on its size. | true | true, false |
| learnTimeLimit | Defines the time limit for the learning process. If 0, no time limit. Format: HH:MM:SS | 0 | TimeSpan |
| scoreMeasure | Defines which measure is used to select the best candidate and to compute the score of a candidate. | RELERROR | RELERROR, INFLUENCE |
| outputRoundsToStdout | If true, the info about the rounds is output not only to the log file at the end of the learning, but also to the stdout during the learning after each round completion. | false | true, false |

Generally, to change the default settings, there are two options, namely:
1. The first is to add the settings in the format ```SETTING_NAME:VALUE``` to the *mlsettings*-command. For instance, if the number of learning rounds should be reduced to 25, allow logarithmic functions and don't want to stop on long learning rounds, the associated command would be:
```mlsettings numberOfRounds:25 learn_logFunction:true stopOnLongRound:false```

2. The second option is to define the settings in a separate text file with each line containing a single setting and its value in the format ```SETTING_NAME VALUE```. This is useful to use the same machine learning settings across several different runs. Then the content of the text file for the example above should look like this:
```
numberOfRounds 25
learn_logFunction true
stopOnLongRound false
```

To load these settings, the command ```load-mlsettings``` (deprecated: ```load_mlsettings```) can be used with the path to the file with the settings as argument. For example: 
```load-mlsettings C:\exampleSettings.txt```

Please note that all the settings that are not stated will automatically be set to the default values. So if the commands are used to change the settings several times during the same run, the previous settings have no impact on the new settings.

#### Setting the non-functional property (NFP)

To learn with the data,  the non functional property that will be used for the learning algorithm has to be set first. Therefore, any property can be used, which was defined previously in the measurement-file. If we use the previous example, we can either use nfp1 or nfp2. To set nfp1 or nfp2 use the ```nfp``` command. Then the appropriate command with the argument is:
```
nfp nfp1
``` 
or
```
nfp nfp2
```

#### Learning with all measurements

To enable learning with all measurements, use ```select-all-measurements true``` command. After that just use the ```learn-splconqueror``` command for learning.
For example:
```
log C:\exampleLog.log
vm C:\exampleModel.xml
all C:\exampleMeasurements.xml
mlsettings numberOfRounds:25 learn_logFunction:true stopOnLongRound:false
nfp nfp1
select-all-measurements true
learn-splconqueror
```

To disable learning with all measurements you can use ```select-all-measurements false```.


***

Deprecated:

Now, we have have enough to learn with all measurements. For this, just use the ```learn-all-splconqueror``` (deprecated: ```learnwithallmeasurements```) command. A .a-script for learning with all measurements at this point, using the examples from above is as follows:
```
log C:\exampleLog.log
vm C:\exampleModel.xml
all C:\exampleMeasurements.xml
mlsettings numberOfRounds:25 learn_logFunction:true stopOnLongRound:false
nfp nfp1
learn-all-splconqueror
```

***

#### Displaying the learning results

The only thing missing for a very basic usage of SPL Conqueror, is displaying the learning results. For this use the ```analyze-learning```-command. This will print the current learning history with the learning error into the specified .log-file. Note, that each command for learning overwrites the previous learning history, so analyze-learning should always be the first command after a command for learning.
Finally, a complete basic .a-script file looks like this:
```
log C:\exampleLog.log
vm C:\exampleModel.xml
all C:\exampleMeasurements.xml
mlsettings numberOfRounds:25 learn_logFunction:true stopOnLongRound:false
nfp nfp1
learnwithallmeasurements
analyze-learning
```

#### Machine-learning parameters

#### Sampling strategies

SPLConqueror also supports learning on a subset of the data. Therefore, one has to set at least one sampling strategy for the binary options first and at least one for the numeric options. Numeric sampling strategies have to always start with ```numeric```(deprecated: ```expdesign```), while binary sampling strategies have to start with ```binary``` (deprecated: no prefix command). In the following, we list all sampling strategies:

| Binary/Numeric | Name  | Description | Command | Example |
| :------------: | :---: | :---------: | :-----: | :-----: |
| Binary | allbinary | Uses all available binary options to create configurations. | ```binary allbinary``` | binary allbinary |
| Binary | featurewise | Determines all required binary options and then adds options until a valid configuration is reached. | ```binary featurewise``` | binary featurewise |
| Binary | pairwise | Generates a configuration for each pair of configuration options. Exceptions: parent-child-relationships, implication-relationships. | ```binary pairwise``` | binary pairwise |
| Binary | negfw | Get one variant per feature multiplied with alternative combinations; the variant tries to maximize the number of selected features, but without the feature in question. | ```binary negfw``` | binary negfw |
| Binary | random | Get certain number of random valid configurations. The binaryThreshold sets the maximum number of configurations. The randomness is simulated by the modulu value. | ```binary random <binaryThreshold> <modulu>``` | binary random 50 3 |
| Numeric | plackettburman | A description of the Plackett-Burman design is provided [here](http://www.jstor.org/discover/10.2307/2332195). | ```numeric plackettburman measurements:<measurements> level:<level>``` | numeric plackettburman measurements:125 level:5 |
| Numeric | centralcomposite | The central composite inscribe design. This design is defined for numeric options that have at least five different values. | ```numeric centralcomposite``` | numeric centralcomposite |
| Numeric | random | This design selects a specified number of value combinations for a set of numeric options. The value combinations are created using a random selection of values of the numeric options. | ```numeric random sampleSize:<size> seed:<seed>``` | numeric random sampleSize:50 seed:2 |
| Numeric | fullfactorial | This design selects all possible combinations of numeric options and their values. | ```numeric fullfactorial``` | numeric fullfactorial |
| Numeric | boxbehnken | This is an implementation of the BoxBehnken Design as proposed in the "Some New Three Level Designs for the Study of Quantitative Variables". | ```numeric boxbehnken``` | numeric boxbehnken |
| Numeric | hypersampling | | ```numeric hypersampling precision:<precisionValue>``` | numeric hypersampling precision:25 |
| Numeric | onefactoratatime | | ```numeric onefactoratatime distinctValuesPerOption:<values>``` | numeric onefactoratatime distinctValuesPerOption:5 |
| Numeric | kexchange | | ```numeric kexchange sampleSize:<size> k:<kvalue>``` | numeric kexchange sampleSize:10 k:3 |
| Both | distribution-aware | Uses distribution-aware sampling to generate sample sets from binary and/or numeric options. | ```hybrid distribution-aware distance-metric:<manhattan> distribution:<uniform> numConfigs:<number/asTW[n]> onlyNumeric:<true/false> onlyBinary:<true/false> seed:<int>``` | hybrid distribution-aware numConfigs:asTW3 |
| Both | distribution-preserving | Uses distribution-preserving sampling to generate sample sets from binary and/or numeric options. | ```hybrid distribution-preserving distance-metric:<manhattan> distribution:<uniform> numConfigs:<number/asTW[n]> onlyNumeric:<true/false> onlyBinary:<true/false> seed:<int>``` | hybrid distribution-preserving numConfigs:asTW3 |


For instance, all binary options and random numeric options with a sample size of 50 and a seed of 3 should be used for learning, the following lines have to be appended to the .a-script:
```
binary allbinary
numeric random sampleSize:50 seed:3
```

If you want to use a hybrid sampling strategy instead, the following line has to be appended to the .a-script:
```
hybrid distribution-aware
```

**Note**: ```allbinary``` in combination with ```fullfactorial``` results in all valid measurements being taken into the sample set.

##### Sampling domain

It also to consider only a subset of the configuration options for aampling. To do this, add the options that should be used in square brackets as additional argument when stating the sampling strategies.
For example:
```
numeric random [numOpt1,numOpt2,numOpt3]
```

#### Learning with sample set

```start```

To learn only with a subset of the measurements, the command ```learn-splconqueror```(deprecated: ```start```) can be used. This command requires having set a binary and a numeric sampling strategy, before executing it.
**Note**: A numeric sampling strategy is only needed if the variability model contains numeric options.

If, for instance, only a subset of the data should be used for learning, the result looks as follows:
```
log C:\exampleLog.log
vm C:\exampleModel.xml
all C:\exampleMeasurements.xml
mlsettings numberOfRounds:25 learn_logFunction:true stopOnLongRound:false
nfp nfp1
binary allbinary
numeric random sampleSize:50 seed:3
learn-splconqueror
analyze-learning
```

#### Cleaning sampling

```clean-sampling```

Due to the different results of the sampling strategies, it is reasonable to try different sampling strategies and parameters for these strategies. To avoid having to start a new run for each sampling strategy combination, SPL Conqueror also supports clearing all strategies.
For this just use the command: ```clean-sampling```
Of course, if someone wants to learn with a subset of the data after clearing the sampling, one has to first set sampling strategies before learning once again.

#### Cleaning learning data

```clean-learning```

Under normal circumstances, SPL Conqueror cleans up the learning data itself. So handling this is usually not required, but if someone wants to forcefully clear all machine learning settings and the learned functions, the command ```clean-learning``` could be used.

#### Subscript

```script <path_to_script>```

Sometimes it makes sense to split up the current .a-script into smaller scripts or run a batch of scripts. For this SPL Conqueror has the ```script``` command.
An example would be as follows:
```script C:\subScript.a```

</details>
<details>
<summary>Additional command-line commands</summary>

#### Printing configurations

```printconfigs <file> <prefix> <postfix>```

With the command ```printconfigs```, all sampled configurations are printed to a persistent file. The command requires a target file as first argument and optionally a prefix or prefix and postfix, that will be printed at the start or end of each configuration, respectively. A special usage of this command is printing all valid configurations of a variability model, using the ```allbinary``` and ```fullfactorial``` sampling strategies.
A short example using printconfigs to print all valid configurations into a text file:
```
vm C:\exampleVM.xml
binary allbinary
numeric fullfactorial
printconfigs C:\allConfigurations.txt prefix postfix
```
Until now, the elements ```outputString```, ```prefix``` and ```postfix``` of the variability model were ignored. These attributes are used by the printconfigs command and printed if the option in question is selected.

#### Option order

```optionorder <firstOption> <secondOption> ...```

In the case, that the options of a configuration should be printed in a certain order, e.g., to use the output as argument for the tested applicatin, the ```optionorder``` command should be used, which sorts all options in the specified order and prints them.
For example: 
```optionorder optionC optionB optionA```

#### Validation set 

```<sampling strategy> validation```

SPL Conqueror offers the possibility to use the validation set.
This validation set is then used to validate the learning results. In case no validation set is specified, the learning set will also be used to validate the results.
To do so, the command ```validation``` has to be added after the sampling strategies.
For example:
```
allbinary validation
expdesign random sampleSize:50 seed:3 validation
```

#### Print settings

```printsettings```

Using the printsettings command, the current machine-learning settings are printed into the .log-file or ,in case yotogehteru didn't set a .log-file, into the console.

#### Writing measurements to .csv-file

```measurementstocsv <file>```

In the case that the measurements should be printed to a .csv-file, the command ```measurementstocsv``` can be used.
For example:
```
measurementstocsv C:\measurementsAsCSV.csv
```

**Note**: The element separator is ```;```, whereas the line separator is ```\n```.

```predict-configs-splconqueror``` (deprecated: ```predict-configurations```)

Predicts the ```nfp``` value of all configurations loaded with the ```all``` command and writes them together with the measured ```nfp``` value and the configuration identifier in a file.

#### Evaluation set

```evaluationset <file>```

By default, SPL Conqueror uses all measurements from the measurements-file for the computation of the error rate.
To change the evaluation set, the command ```evaluationset``` can be used.
The file can be either a .csv-file or a .xml-file.
For example:
```
evaluationset C:\evaluationMeasurements.xml
```

**Note**: The format specified in the evaluation-file is the same as in the measurements-file.

#### Recover

```resume-log <abortedAFile>```

In the case that SPL Conqueror aborts unexpectedly, for instance because of a system crash, in a lot of cases the learning-process can be resumed. 
To do so, a new .a-script has to be created, which contains the ```resume-log``` command with the .a-script that aborted as argument.
For example: 
```
resume-log C:\abortedScript.a
```

</details>
<details>
<summary>Exemplary Script-Files</summary>
A .a-file contains the configuration of SPL Conqueror.
If one is interested in using all measurement-data, the following .a-file could be used:

```
# Lines containing a comment begin with '#'

# The log command and the destination file, where the learning progress should be written to
log ./learnOutput.txt

# The machine-learning settings for configuring different options for machine-learning. These are described in the documentation more precisely
mlsettings bagging:False stopOnLongRound:False parallelization:False lossFunction:RELATIVE useBackward:False abortError:10 limitFeatureSize:False featureSizeTreshold:7 quadraticFunctionSupport:True crossValidation:False learn_logFunction:True numberOfRounds:70 backwardErrorDelta:1 minImprovementPerRound:0.25 withHierarchy:False

# The path to the variability model (feature model)
vm ./VariabilityModel.xml

# The file containing all measurements needed for machine-learning
all ./measurements.xml

# The non-functional property, which was measured.
# Note that every configuration in the measurements-file needs a data-row with the attribute 'columname=Performance'
nfp Performance

# Learns with all configurations given in the measurements-file.
# Note that 'start' is not needed in combination with 'learnwithallmeasurements'
learnwithallmeasurements

# Cleans the sample set.
# Note that this command is needed if multiple different sampling sets are computed in one run of SPL Conqueror
clean-sampling

```

In SPL Conqueror, multiple different sampling strategies for binary and numeric features are implemented and can be used in the .a-file as follows:

```
# The first lines are the same as in the previous example
log ./learnOutput.txt
mlsettings bagging:False stopOnLongRound:False parallelization:False lossFunction:RELATIVE useBackward:False abortError:10 limitFeatureSize:False featureSizeTreshold:7 quadraticFunctionSupport:True crossValidation:False learn_logFunction:True numberOfRounds:70 backwardErrorDelta:1 minImprovementPerRound:0.25 withHierarchy:False
vm ./VariabilityModel.xml
all ./measurements.xml
nfp Performance

# Here, the binary sampling strategy FeatureWise (FW) is selected
binary featurewise

# The sampling strategy Plackett-Burman for numeric options is selected
numeric plackettburman measurements:125 level:5

# Print configurations selected by the sampling strategies in the file 'samples.txt'
printconfigs ./samples.txt

# Start learning using the sampled configurations
learn-splconqueror

# Predicts the performance value of all configurations of the measurements file using the learned performance-influence model
predict-configs-splconqueror

```

If multiple (different) .a-files should be executed, a super-script can be created as follows:
```
# Calls the first script
script ./scriptA.a
# Removes all variables related to the invocation of the first script
clean-global
# Calls the second script
script ./scriptB.a
clean-global
```

See the previous chapters for a more detailed description of the commands.
For further examples, see the directory 'Example Files'.
</details>
