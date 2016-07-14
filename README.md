Agile CRM iOS SDK
=================

[Agile CRM] (https://www.agilecrm.com/) is a new breed CRM software with sales and marketing automation.

Table of contents
---------------

**[Requirements](#requirements)**

**[API's details](#apis-details)**

**[1 Contact](#1-contact)**
  * [1 To create a contact](#11-to-create-a-contact)
  * [2 To fetch contact data](#12-to-fetch-contact-data)
  * [3 To delete a contact](#13-to-delete-a-contact)
  * [4 To update a contact](#14-to-update-a-contact)
  * [5 Add tag to a contact](#15-add-tag-to-a-contact)
  * [6 Remove tag to a contact](#16-delete-tag-to-a-contact)

**[2. Deal (Opportunity)](#2-deal)**
  * [1 To create a deal](#21-to-create-a-deal)
  * [2 To update a deal](#22-to-update-a-deal)
  * [3 Get deal by ID](#23-get-deal-by-id)
  * [4 Delete deal by ID](#24-delete-deal-by-id)

**[3. Note ](#3-note)**
  * [1 To create a note](#31-to-create-a-note)
  * [2 To update a note](#32-to-update-a-note)
  * [3 Get note by ID](#33-get-note-by-id)
  * [4 Delete note by ID](#34-delete-note-by-id)
 
**[4. Task ](#4-task)**
  * [1 To create a task](#41-to-create-a-task)
  * [2 To update a task](#42-to-update-a-task)
  * [3 Get task by ID](#43-get-task-by-id)
  * [4 Delete task by ID](#44-delete-task-by-id)

Requirements
------------

1. Xcode with minimum 6.0 and OS X 10.10 

2. iOS 8.3 tested platform

3. Setting domain name and API key

![Finding domain name, email and API key] (https://raw.githubusercontent.com/agilecrm/c-sharp-api/master/AgileCRMapi.png)

In the above image, API key is present at the "API & Analytics" tab at `https://mycompany.agilecrm.com/#account-prefs`.

        Domain name : mycompany
        API key     : myagilecrmapikey
        Email       : myagilecrmemail

So you have to update your `https://github.com/agilecrm/ios-sdk/blob/master/iOSFile/ApiTest/ApiTest/ViewController.m`

	
```javascript
 AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
```

API's details
-------------
## 1. Contact
#### 1.1 To create a contact 

- [**Acceptable request representation for contact**](https://github.com/agilecrm/rest-api#acceptable-request-representation)

```javascript
- (IBAction)addContact;
{
    NSMutableArray *properties = [[NSMutableArray alloc] init];
    Field *f = [[Field alloc] initWithName:@"first_name" andValue:@"Sam" andType:@"SYSTEM"];
    [properties addObject:f];
    
    NSArray *tags = [[NSArray alloc] initWithObjects:@"Tag1", @"Tag2", nil];

    NSNumber *leadScore = [[NSNumber alloc] initWithInt:20];
    NSNumber *starValue = [[NSNumber alloc] initWithInt:30];
    Contact *contact = [[Contact alloc] initWithProperties:properties andTags:tags andLeadScore:leadScore andStarValue:starValue];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] add:contact onCompletion:^(Contact *contact) {
        self.result.text = [NSString stringWithFormat:@"id %@", contact.contactId];
        NSLog (@"id %@", contact.contactId);
        for (Field *yourVar2 in contact.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 1.2 To fetch contact data

###### by id

```javascript
- (IBAction)specificContact;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] getContactWithId:@"5655869022797824" onCompletion:^(Contact *contact) {
        for (Field *yourVar2 in contact.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
            self.result.text = [NSString stringWithFormat:@"%@ - %@", yourVar2.name, yourVar2.value];
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```
###### by email

```javascript
- (IBAction)specificEmailContact;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] getContactWithEmail:@"ff2@gmail.com" onCompletion:^(Contact *contact) {
        for (Field *yourVar2 in contact.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
            self.result.text = [NSString stringWithFormat:@"%@ - %@", yourVar2.name, yourVar2.value];
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 1.3 To delete a contact

```javascript
- (IBAction)deleteContactById;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] remove:@"5754903989321728" onCompletion:^(BOOL success) {
        if (success) {
            NSLog (@"REMOVED");
            self.result.text = @"REMOVED 5754903989321728";
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 1.4 To update a contact 

- [**Acceptable request representation for contact**](https://github.com/agilecrm/rest-api#acceptable-request-representation-1)

```javascript
    NSMutableArray *properties = [[NSMutableArray alloc] init];
    Field *f = [[Field alloc] initWithName:@"first_name" andValue:@"Phelipe2" andType:@"SYSTEM"];
    [properties addObject:f];
    
    NSArray *tags = [[NSArray alloc] initWithObjects:@"Tag1", @"Tag2", nil];
    
    NSNumber *leadScore = [[NSNumber alloc] initWithInt:20];
    NSNumber *starValue = [[NSNumber alloc] initWithInt:30];
    Contact *contact = [[Contact alloc] initWithProperties:properties andTags:tags andLeadScore:leadScore andStarValue:starValue];
    contact.contactId = @"5754903989321728";
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] update:contact onCompletion:^(Contact *contact) {
        for (Field *yourVar2 in contact.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
            self.result.text = [NSString stringWithFormat:@"%@ - %@", yourVar2.name, yourVar2.value];
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
```

#### 1.5 Add tag to a contact

```javascript
     NSArray *tags = [[NSArray alloc] initWithObjects:@"Tag1", @"Tag2", nil];
    
    
    Contact *contact = [[Contact alloc] Tags:tags];
    contact.contactId = @"5754903989321728";
   AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] add:contact onCompletion:^(Contact *contact) {
        for (Field *yourVar2 in contact.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
            self.result.text = [NSString stringWithFormat:@"%@ - %@", yourVar2.name, yourVar2.value];
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
```

#### 1.6 Delete tag to a contact

```javascript
     NSArray *tags = [[NSArray alloc] initWithObjects:@"Tag1", @"Tag2", nil];
    
    
    Contact *contact = [[Contact alloc] Tags:tags];
    contact.contactId = @"5754903989321728";
   AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] delete:contact onCompletion:^(Contact *contact) {
        for (Field *yourVar2 in contact.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
            self.result.text = [NSString stringWithFormat:@"%@ - %@", yourVar2.name, yourVar2.value];
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
```

## 2. Deal
#### 2.1 To create a deal 

- [**Acceptable request representation for contact**](https://github.com/agilecrm/rest-api#acceptable-request-representation-9)

```javascript
- (IBAction)addDeal;
{
    NSArray *contatIDS = [[NSArray alloc] initWithObjects:@"585658565856", @"578988989988", nil];

    NSNumber *expectedvalue = [[NSNumber alloc] initWithInt:20];
    NSNumber *probability = [[NSNumber alloc] initWithInt:30];
    Deal *deal = [[Deal alloc] initWithProperties:properties andTags:tags andLeadScore:leadScore andStarValue:starValue];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager dealAPI] add:deal onCompletion:^(Deal *deal) {
        self.result.text = [NSString stringWithFormat:@"id %@", deal.dealId];
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 2.2 To update a deal 

- [**Acceptable request representation for contact**](https://github.com/agilecrm/rest-api#acceptable-request-representation-10)

```javascript
- (IBAction)updateDeal;
{
    NSArray *contatIDS = [[NSArray alloc] initWithObjects:@"585658565856", @"578988989988", nil];

    NSNumber *expectedvalue = [[NSNumber alloc] initWithInt:20];
    NSNumber *probability = [[NSNumber alloc] initWithInt:30];
    Deal *deal = [[Deal alloc] initWithProperties:properties andTags:tags andLeadScore:leadScore andStarValue:starValue];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager dealAPI] add:deal onCompletion:^(Deal *deal) {
        self.result.text = [NSString stringWithFormat:@"id %@", deal.dealId];
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 2.3 Get deal by ID


```javascript
- (IBAction)specificContact;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] getDealWithId:@"5655869022797824" onCompletion:^(Deal *deal) {
        for (Field *yourVar2 in deal.properties) {
            NSLog (@"%@ - %@", yourVar2.name, yourVar2.value);
            self.result.text = [NSString stringWithFormat:@"%@ - %@", yourVar2.name, yourVar2.value];
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 2.4 Delete deal by ID


```javascript
- (IBAction)deleteDealById;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] remove:@"5754903989321728" onCompletion:^(BOOL success) {
        if (success) {
            NSLog (@"REMOVED");
            self.result.text = @"REMOVED 5754903989321728";
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

## 3. Note
#### 3.1 To create a Note 

```javascript
- (IBAction)addNote;
{
    NSArray *contactIDS = [[NSArray alloc] initWithObjects:@"585658565856", @"578988989988", nil];

    NSNumber *subject = [[NSNumber alloc] initWithString:hello];
    NSNumber *description = [[NSNumber alloc] initWithString:this is note];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager dealAPI] add:note onCompletion:^(Note *note) {
        
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 3.2 To update a note 


```javascript
- (IBAction)updateNote;
{
    NSArray *contactIDS = [[NSArray alloc] initWithObjects:@"585658565856", @"578988989988", nil];

    NSNumber *subject = [[NSNumber alloc] initWithString:hello];
    NSNumber *description = [[NSNumber alloc] initWithString:this is note];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager dealAPI] add:note onCompletion:^(Note *note) {
        
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 3.3 Get note by ID


```javascript
- (IBAction)getNotes;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] getDealWithId:@"5655869022797824" onCompletion:^(Deal *deal) {
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 3.4 Delete note by ID


```javascript
- (IBAction)deleteNoteById;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] remove:@"5754903989321728" onCompletion:^(BOOL success) {
        if (success) {
            NSLog (@"REMOVED");
            self.result.text = @"REMOVED 5754903989321728";
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

## 4. Task
#### 4.1 To create a task 

```javascript
- (IBAction)addTask;
{
    NSArray *contactIDS = [[NSArray alloc] initWithObjects:@"585658565856", @"578988989988", nil];

    NSNumber *name = [[NSNumber alloc] initWithString:hello];
    NSNumber *owner = [[NSNumber alloc] initWithString:this is note];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager dealAPI] add:note onCompletion:^(Note *note) {
        
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```
#### 4.2 To update a task 

- [**Acceptable request representation for contact**](https://github.com/agilecrm/rest-api#acceptable-request-representation-10)

```javascript
- (IBAction)updateTask;
{
    NSArray *contactIDS = [[NSArray alloc] initWithObjects:@"585658565856", @"578988989988", nil];

    NSNumber *name = [[NSNumber alloc] initWithString:hello];
    NSNumber *owner = [[NSNumber alloc] initWithString:this is note];
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager dealAPI] add:note onCompletion:^(Note *note) {
        
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 4.3 Get task by ID


```javascript
- (IBAction)getTasks;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] getDealWithId:@"5655869022797824" onCompletion:^(Deal *deal) {
        
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```

#### 4.4 Delete task by ID


```javascript
- (IBAction)deleteTaskById;
{
    AgileCRMManager *manager = [[AgileCRMManager alloc] initWithDomain:@"your_domain" andKey:@"your_rest_api_key" andEmail:@"your_email"];
    [[manager contactAPI] remove:@"5754903989321728" onCompletion:^(BOOL success) {
        if (success) {
            NSLog (@"REMOVED");
            self.result.text = @"REMOVED 5754903989321728";
        }
    } onError:^(NSError *originalError) {
        NSLog(@"Error");
    }];
    
}
```