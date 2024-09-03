# Detecting-covert-disruption
This project examines conversational properties such as conversational actions in online forum interaction to detect covert disruptive (trolling) behavior in a conversation.

![image](https://github.com/user-attachments/assets/b8ef7098-e4f7-4157-a33d-ffad52ed0949)

Model card as suggested in Mitchell et al. (2019).


## Data input for feature extraction

This project requires conversational data as input in JSON format for feature extraction, with the following structure:

* conversation_id: a key at root level for each conversation included in the dataset

For each conversation:

* messages: a key pointing to list of messages that are part of the same conversation (a conversation in a threaded structure is initiated by a root-level comment, formed by all comments that are part of the same reply tree initiated by the root-level comment).

Message items in the list with the following information (keys):

* Message_text: comment in conversation as string.
* Author_ID: unique identifier for comment author.
* Author: pseudonym or altered nickname if available.
* Message_ID: unique identifier for comment.
* Conversation_id: conversation_id identifier pointing back to root level conversation key.
* Reply_to_msg_ID: Message_ID identifier of the parent comment the comment is replying to, if applicable.
* Probabilities: classification probabilities for conversational actions (see below).
* Toxicity: toxicity scores per Perspective API.
* Date_and_time: date and time when comment was posted.



## Usage

Before feature extraction and trolling classification, several steps are required:

* Toxicity classification using Perspective API.
* Politeness strategies and rhetorical prompt classification for each conversation, similarly to Zhang et al. (2018). Use a JSON file with conversation_id at root level for each conversation, saving each feature under a separate key (`file[conversation_id][prompt]` AND `file[conversation_id][polite]`).
* Download blacklist words file provided by Liu et al. (2005).
* Classify all comments in data for conversational action probabilities, either using MNLI (Yin et al., 2019) or a suitable approach, with the following classes: `["a question", "a challenge", "an accusation", "a request for action", "a proposal", "a statement", "an appreciation", "a denial", "an apology", "an acceptance"]`
* Classify all comments with probabilities for the following additional categories, e.g. with MNLI: `["a disagreement", "an agreement", "a negative reaction", "a positive reaction"]`

Feature extraction:

* Use `Extract_action_features_and_asymmetries_in_conversations.ipynb` for extracting conversation features.
* Output is a .csv file with conversation features as vectors.

Trolling classification:

* We have described the implementation of the trolling conversations classification model in our paper (Paakki et al., 2024). Please see the paper in order to replicate the model. However, unfortunately, due to potential risks and ethical issues, following our [University's guidelines](https://www.aalto.fi/sites/g/files/flghsv161/files/2022-05/Aalto_University_Open_Science_and_Research_Policy_050222.pdf), we cannot publish the model openly.

## Data

* At the time when the data for this project was originally collected, policies and rules of our data sources were different. Similarly, University policies regarding privacy and responsible open science have been since updated. Due to these changes, in order to follow [University's guidelines of responsible open science](https://www.aalto.fi/sites/g/files/flghsv161/files/2022-05/Aalto_University_Open_Science_and_Research_Policy_050222.pdf), we are unable to openly share the data. Please get in touch with the authors for further details.

***

If you use the resources provided here, please use the recommended citation:

`Henna Paakki, Heidi Vepsäläinen, Antti Salovaara, and Bushra Zafar. 2024. Detecting Covert Disruptive Behavior in Online Interaction by Analyzing Conversational Features and Norm Violations. ACM Trans. Comput.-Hum. Interact. 31, 2, Article 20 (April 2024), 43 pages. https://doi.org/10.1145/3635143`


