# AI Forex Assistant (GenAI - Copilot)
This repository contains a step-by-step guide of how to create an AI Forex Assistant using Microsoft Power Platform tools like Power Apps and Power Automate, making use of latest Generative AI technology such as Copilot. The goal is to create a user dashboard where a foreign exchange pair evolution is displayed, together with supporting data.

In this specific case and for illustration purposes, the selected foreign exchange pair has been the CHF/EUR pair. In addition, historical Gross Domestic Product (GDP) growth rates and interest rates of Switzerland and the European Union are used as supporting data. Not only that, but the user can also ask the AI Forex Assistant to predict next month's exchange rate. Thanks to a trained Machine Learning model on historical data, the AI Forex Assistant is then able to estimate the future value considering market trends.

The user interface and dashboard of the application can be visualized in the following image:<br><br>
![Image](/images/dashboard_final.PNG)

## Index
This guide follows a series of steps to complete the development of the AI Forex Assistant application:

**1.- Create and populate Data Tables using Copilot in Power Apps.**

**2.- Design dashboard user interface with the help of Copilot in Power Apps.**

**3.- Generate and train a Custom Prediction Model in AI Builder.**

**4.- Create a flow in Power Automate with Copilot to predict future values.**

**5.- Integrate the Power Automate flow into Power Apps and test the application.**<br><br>


## 1.- Create and populate Data Tables using Copilot in Power Apps
The first thing that we need to do is to ask Copilot what kind of App we want to build in Power Apps. In any interaction with Copilot, it is essential to make use of AI prompt engineering in order to get the best out of the trained LLM behind it. In general, there are some good practices to follow when interacting with Copilot in Power Apps and Power Automate:

* Specify both what and how. Remember to mention both what you want and how you want it.

* Support the action or instruction of your prompt with relevant content or cues that influence the action.

* Specify context. Context matters, the more you can specify like domain, topic, etc. the better.

* Provide information about the format of the output.

* Do not expect the same exact output from the same prompt always. Copilot and all LLM models are non-deterministic so the output might vary each time.

In our case, we will ask Copilot that we want to build an application to display CHF to EUR exchange rates and supporting data like CH GDP Growth, EU GDP Growth, CHF Interest Rate and EUR Interest Rate, where each value corresponds to a month:

![Image](/images/copilot_create_app.PNG)
<br><br>
As a result, Copilot generates a Data Table to meet our needs, containing random values:

![Image](/images/copilot_table_1.PNG)
<br><br>
As you can see, we get a warning message saying that "A text primary name column is required". Moreover, there are additional modifications that we want to perform on the table. One of the greatest things about Copilot is that we can have a conversation with it, iteratively interact and provide one instruction after another until we are satisfied with the result. Apart from converting the month column into the primary name column, we are also going to add more rows and modify the month value to go from 08-2019 to 10-2023. This is the sequence of the correct prompts:

1- "Please add 46 more rows."

2- "The month column should go from 01/08/2019 to 01/10/2023."

3- "Convert the Month column into text. The values should display the month and the year only."

After this conversation, the table should look like this:

![Image](/images/copilot_table_2.PNG)

At this point, once we populate the table with the correct historical data, we can then create the app and continue with the user interface design. Note that the historical data has been collected from trusted sources such as the European Central Bank (ECB) and the Swiss National Bank (SNB). The table is stored as a Dataverse table in Power Apps.

<br><br>
## 2.- Design dashboard user interface with the help of Copilot in Power Apps
Now that the app has been created in Power Apps, we need to work on the user interface of the main screen. By default, the app will have a data table screen at first:

![Image](/images/dashboard_1.PNG)
<br><br>
So the first thing we need to do is to remove that data table screen and add the line charts of CHF to EUR, GDP Growth and Interest Rate. For that, we are going to delete the ScreenContainer1 element on the left pane and then ask Copilot how we can add our three line charts of interest, with the prompt "Suggest me how to add a line chart". Note that once we follow Copilot's instructions, we have to customize the data, title, legends and color of each chart:

![Image](/images/dashboard_2.PNG)
<br><br>
Next, we are going to ask Copilot to Add a button with a text that says "Predict Future Exchange Rate". Likewise, we are going to conveniently let Copilot add three text labels. After placing the new elements in their respective positions and customizing their text format, we are going to create a global variable called varFutureExchangeRate, which will be linked to our predicted future exchange rate value. The final dashboard user interface should look like this:

![Image](/images/dashboard_3.PNG)
<br><br>
<br><br>
## 3.- Generate and train a Custom Prediction Model in AI Builder
We are now finished with the design of the dashboard user interface and we can move on to the next step, which involves generating and training a Custom Prediction Model in AI Builder inside Power Apps. If we go to the AI Hub section from the Power Apps homepage and access the AI models (it can also be done from Power Automate), we are presented with a list of prebuilt and custom models that can be used within Power Apps and Power Automate:

![Image](/images/model_1.PNG)


For this use case, we are going to select and create a "Predict future outcomes from historical data" custom model. The difference between prebuilt models and custom models is that custom models need to be trained with your own dataset. For the purpose of creating this guide, we are going to use our very simple dataset, consisting of 51 samples of historical data (the minimum number of samples for any custom model is 50). Please refer to the disclaimer at the bottom of this guide to read more about the model employed for this application.

As a first step, we must select the CHF to EUR column in our Currency Data table as the historical outcomes the model should study to predict future values. Next, we are going to select which are the feature inputs of the model that will influence the prediction. In this case, we are going to select the supporting historical data that is directly correlated to future exchange rates, that is, the CH and EU GDP Growth, together with the CHF and EUR Interest Rates. Do not forget to include the time context of our data by including the Month column as well:

![Image](/images/model_2.PNG)

![Image](/images/model_3.PNG)
<br><br>
After that, we can skip the filtering step and continue with the model training. With such a small dataset, the training should not take more than 5 minutes. Finally, when the training ends, we need to publish the model if we want to access it in Power Automate. Note that in case we want to change the settings of the published model anytime, we can always do it and retrain the model to adapt to the new configuration.

![Image](/images/model_4.PNG)
<br><br>
<br><br>
## 4.- Create a flow in Power Automate with Copilot to predict future values
Power Automate is a very powerful tool that allows to quickly automate tasks, consisting of various steps, into one single flow. The created flow can be a Desktop flow, Cloud flow or Business process flow depending on the target application. In our case, we need to create a Cloud flow, which will be triggered from our dashboard user interface when hitting the "Predict Future Exchange Rate" button. Since the trigger will happen from Power Apps, this will be an instant cloud flow. Similarly, automated cloud flows or scheduled cloud flows can also be used in Power Automate for different types of triggering methods.

What is more, since Copilot has been recently introduced in Power Automate, these steps and flows can now be created using Generative AI techniques, through Prompt Engineering. Therefore, we are going to describe to Copilot what kind of task we want to automate in Power Automate. With our information, Copilot will already know that the new flow needs to be an instant cloud flow. This is the employed Copilot prompt and the corresponding output:

![Image](/images/automate_1.PNG)
![Image](/images/automate_2.PNG)

In appearance, the suggested flow seems to meet our requirements, but it is far from perfect. Do not get frustrated, you can always ask Copilot to suggest a different flow or you can have an iterative conversation with it in order improve your flow. After reviewing the connections, we are going to create the flow and start modifying it. We notice that the predict action does not allow to add AI models, so we tell Copilot to remove it and directly ask how we can add an action from AI Builder. After following the suggested instructions of Copilot, the flow should look like this:

![Image](/images/automate_3.PNG)
<br><br>
Next, we need to send the predicted output back to Power Apps. Thus, we ask Copilot, "How can I respond to Power Apps?". Following its suggestion, we add the "Respond to a Power App or flow" action, like this:

![Image](/images/automate_4.PNG)
<br><br>
At this point, we need to configure each of the blocks in our flow. Firstly, we open the "PowerApps (V2)" block and add all the input data that will come from Power Apps. Likewise, we are going to add an output at the "Respond to a PowerApp or flow" block that will be linked to the response of our AI prediction model (use the lightning icon to do this):

![Image](/images/automate_5.PNG)
![Image](/images/automate_6.PNG)
<br><br>
Regarding the "Predict" block, we are going to choose the model created in Step 3 and select all of the five advanced parameters (inputs) of the model. Make sure to connect each advanced parameter with the data from the previous block using the lightning icon.

![Image](/images/automate_7.PNG)

The flow is now ready to be used. Nevertheless, before we prepare the flow interface on the Power Apps side, it is highly adviceable to test the created flow. We can do this by using the "Test this flow" functionality provided by Copilot. We will perform a manual test by introducing static input data:

![Image](/images/automate_8.PNG)

Lastly, we run the flow and observe the results of the test. Power Automate provides runtime information about each block, such as the required execution time, the received inputs or the produced outputs.

![Image](/images/automate_9.PNG)
<br><br>
In this case, we can see that it took 10 seconds to execute the prediction and that the predicted CHF/EUR exchange rate in the test has been 1.03, matching the ground truth. We are now ready to move on to the final step of our guide.
<br><br>
<br><br>
## 5.- Integrate the Power Automate flow into Power Apps and test the application
Going back to Power Apps, we need to import the newly created and tested Power Automate flow. For that, we are going to go to the Power Automate section in the left pane menu and add our flow:

![Image](/images/integrate_1.PNG)
<br><br>
After that, we are going program the flow to run every time we click the "Predict Future Exchange Rate" button. This can be done using Power Platform specific syntax by calling the flow Run() function as part of the button OnSelect formula. The Run() function needs to be fed with its five inputs: CH GDP Growth, EU GDP Growth, CHF Interest Rate, EUR Interest Rate and Month. Therefore, we make sure to pass the last value of each of this parameters from our data table. Additionally, we want to copy the output of the flow to the global variable "varFutureExchangeRate" using the Set() function, so that it gets displayed in the text label of our dashboard user interface. The final formula can be found hereafter:

![Image](/images/integrate_2.PNG)
<br><br>

As a final step, we are going to save our application and hit the play button in Power Apps to preview and test the app. The application preview should be generated without any errors and the Forex Predictor should be able to predict the future CHF/EUR value after pressing the "Predict Future Exchange Rate" button, like this:

![Image](/images/dashboard_final.PNG)
<br><br>
It is always a good practice to check if the flow was run correctly. We verify that in the Run History within Power Automate, and fortunately we see that everything worked out like a charm:

![Image](/images/integrate_3.PNG)
<br><br>
**Congratulations! The development of the application is now finalized!**

<br><br>
## Disclaimer
The purpose of this guide and applicaton is not to predict future foreign exchange values as accurately as possible, but to focus on the steps followed and functionalities available in Power Apps and Power Automate with the assistance of Generative AI, via Copilot. The author is aware that the predictor model is likely to underfit due to the size of the training dataset. Accuracy and reliability of the prediction could be improved in several ways (using a larger training dataset to avoid underfitting, increasing decimal places on data, adding extra supporting data such as Income Growth Rate, connecting the data tables to an API that provides real-time financial information, etc.), but that is out of the scope of this guide.
