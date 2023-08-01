# ServiceDesk-Chatbot
The "ServiceDesk Bot" is a Python program designed to provide basic support and assistance for users facing common IT-related issues. The bot engages users in a conversation to help them with password resets, LDAP password issues, and VPN problems. It utilizes regular expressions to identify user intents and generates appropriate responses based on the identified intent. The bot interacts with users through a series of questions and provides helpful support links for resolving their issues. Users can exit the conversation using predefined exit commands. The ServiceDesk Bot aims to facilitate self-service support and ticket creation for IT-related problems, enabling users to obtain assistance quickly and efficiently.

Program code:
```
# Importing Regex & Random Libraries
import re
import random

# Creating Servicedesk Bot
class ServicedeskBot:
    # Tuple of Negative Responses
    negative_responses = ("no", "nope", "nah", "naw", "not a chance", "sorry no", "no thanks", "no thank you")

    # Tuple of Keywords to Exit Conversation
    exit_commands = ("quit", "pause", "exit", "goodbye", "bye", "later", "I'm done", "see you later", "peace", "talk to you later")

    # Random Questions
    random_questions = ("Why are you here? ", "Are there questions? ", "How can we help you?")

    # Class Initializer
    def __init__(self):
        self.reresponses = {"forgot_password_intent": r"(?=.*\b(?:help|forgot|reset(?:ting)?)\b)(?=.*\bpassword\b)(?!.*\bldap\b).*$", "ldap_password_intent": r"(?=.*\b(?:ldap|LDAP)\b)(?:(?=.*\bhelp\b)|(?=.*\bpassword\b)|(?=.*\b(?:reset|resetting|forgot)\b)).*$", "vpn_problem_intent": r"(?i)\bVPN\b"}

    # Defining Greeting Function
    def greeting(self):
        self.name = input("Hello! Thanks for reaching out to the ServiceDesk Bot. Before we proceed with helping you, what is your name? ")

        help_needed = input(f"Nice to meet you {self.name}! It looks like you are trying to submit a ServiceDesk ticket. Is that correct?  ")

        if help_needed in self.negative_responses:
            print("Sounds good! Please feel free to reach back out if anything else is needed!")
        return self.chat()
    
    # Defining Function to Exit ServiceDesk Chatbot
    def exit_bot(self, reply):
        for words in self.exit_commands:
            if words in reply:
                print("Thanks for using the ServiceDesk Chatbot. Until next time!")
                return True

    # Defining Function to Initiate ServiceDesk Bot Chat
    def chat(self): 
        reply = input(random.choice(self.random_questions)).lower()

        while not self.exit_bot(reply):
            reply = input(self.chat_reply(reply))
        return
    
    # Defining Function to Handle Conversation
    def chat_reply(self, reply):
        reply = reply.strip().lower()
        matched_intent = None
        for intent, regex_pattern in self.reresponses.items():
            found_match = re.search(regex_pattern, reply)
            if found_match:
                matched_intent = intent
                break

        if matched_intent == "ldap_password_intent":
            return self.ldap_password_intent()
        elif matched_intent == "forgot_password_intent":
            return self.forgot_password_intent()
        elif matched_intent == "vpn_problem_intent":
            return self.vpn_problem_intent()
        else:
            return self.no_match_intent()

    # Defining Function: Handling Password Intent  
    def forgot_password_intent(self):
        reply = input("It looks like you're running into some issues with your password. Please follow the steps highlighted in the article below to resolve the mentioned issue: \n\nhttps://support.apple.com/en-us/HT201487 \n\nIf additional support is required please respond yes to this message. ")

        reply = reply.strip().lower()
        
        if reply == "yes":
            return self.no_match_intent()
        
    
    #Defining Function: Handling LDAP Intent
    def ldap_password_intent(self):
        reply = input("I'm sorry to hear you're running into issues with your LDAP password. Unfortunately, IT would not be the ones managing this resource. For the best support, please reach out to the GTOC team via their service portal below: \n\nhttps://support.apple.com/en-us/HT201487 \n\nIf additional support is required, please respond yes to this message. ")
        
        reply = reply.strip().lower()
        
        if reply == "yes":
            return self.no_match_intent()
    
    # Defining Function: VPN Problem Intent
    def vpn_problem_intent(self):
        reply = input("I'm sorry to hear you're running into issues with your VPN. To help resolve this issue, can we please have you run the steps mentioned in the attached article below: \n\nhttps://support.apple.com/en-us/HT201487 \n\nIf additional support is required, please respond yes to this message. ")

        reply = reply.strip().lower()
        
        if reply == "yes":
            return self.no_match_intent()
    
    # Defining Function: No Match Intent
    def no_match_intent(self):
        reply = input("Thanks for your response. Unfortunately, at this time, I am unable to provide any support articles to assist with your request. I've gone ahead and created the following IT ticket (IT-123456). Please allow a couple minutes for a technician to reach out via ServiceDesk. Thanks for using the ServiceDesk Bot. Please reply \"Quit\" to end this interaction.")
        
        reply = reply.strip().lower()

        return self.exit_bot(reply)
        

# Creating an Instance of ServicedeskBot below:
my_bot = ServicedeskBot()
my_bot.greeting()
```
Expected Outcome:
```
Hello! Thanks for reaching out to the ServiceDesk Bot. Before we proceed with helping you, what is your name? Christian

Nice to meet you Christian! It looks like you are trying to submit a ServiceDesk ticket. Is that correct?  Yes

Why are you here? My VPN doesnt work. I need help ASAP.

I'm sorry to hear you're running into issues with your VPN. To help resolve this issue, can we please have you run the steps mentioned in the attached article below: 

https://support.apple.com/en-us/HT201487 

If additional support is required, please respond yes to this message. yes

Thanks for your response. Unfortunately, at this time, I am unable to provide any support articles to assist with your request. I've gone ahead and created the following IT ticket (IT-123456). Please allow a couple minutes for a technician to reach out via ServiceDesk. Thanks for using the ServiceDesk Bot. Please reply "Quit" to end this interaction.quit

Thanks for using the ServiceDesk Chatbot. Until next time!
```
