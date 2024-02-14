**Part One**

A failure-inducing input for the buggy program: 
```
public void testReverseInPlace() {
    int[] input2 = {1,2,3};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{3,2,1}, input1);
```

An input that doesn't induce a failure:
```
public void testReverseInPlace() {
    int[] input1 = { 7 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 7 }, input1);
```
The symptom: 
 ![Image](testfail1.png)

The bug:
```
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

Fixed: 
```
  static void reverseInPlace(int[] arr) {
    for (int i = 0; i < arr.length / 2; i += 1) {
      int placeholder = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = placeholder;
    }
  }
```
In the original code, the reversed values are not saved for the next iteration. This causes the final array to repeat numbers because certain values in the array are not updated correctly. In my code, we only iterate up to ```arr.length / 2``` to avoid the issue in the original code. I then store ```arr[i]``` in the temporary placeholder, which will later be set to ```arr[arr.length - i - 1]``` to "reverse" the array.

**Part two**
1. ```grep -r "example" .```
-  ```grep -r "transliteration" ./technical```
-  Outputs:
chapter-2.txt           we use its transliteration, e.g.,"al Qida" instead of al Qaeda.
chapter-13.5.text             40. Among the more important problems to address is that of varying transliterations
-  ```grep -r "Nostalgia" ./technical```
-  Outputs:
chapter-2.txt             Nostalgia for Islam's past glory remains a powerful force.
This command is useful if you want to search for a specific word, and when there are hundreds of files to look through. 

2. ```grep -v "example" file.txt```
- ```grep -v "the" chapter-1.txt```
- Output (Omitted some of the output because too long): 
    FAA: Yes.

    NEADS: On its way towards Washington?


    NEADS: Okay.



    NEADS: He-American 11 is a hijack?

    FAA: Yes.

    NEADS: And he's heading into Washington?

    FAA: Yes. This could be a third aircraft.
- ```grep -v "a" chapter-1.txt```
- Output (Omitted some of the output because too long): 
Center: Do you know who he is?

This command is useful if you want to search for lines in a specific line that don't contain a specific word. 


3. ```grep -e "pattern1" -e "pattern2" file.txt```
- ```grep -e "6:45" -e "7:40" chapter-1.txt```
- Output:
Atta and Omari arrived in Boston at 6:45. Seven minutes later, Atta apparently took a call from Marwan al Shehhi, a longtime colleague    
who was at another terminal at Logan Airport. They spoke for three minutes.
    Between 6:45 and 7:40, Atta and Omari, along with Satam al Suqami, Wail al Shehri, and Waleed al Shehri, checked in and boarded American  
Airlines Flight 11, bound for Los Angeles. The flight was scheduled to depart at 7:45.
    While Atta had been selected by CAPPS in Portland, three members of his hijacking team-Suqami, Wail al Shehri, and Waleed al Shehri-were  
selected in Boston. Their selection affected only the handling of their checked bags, not their screening at the checkpoint. All five men     
cleared the checkpoint and made their way to the gate for American 11. Atta, Omari, and Suqami took their seats in business class (seats 8D,  
8G, and 10B, respectively). The Shehri brothers had adjacent seats in row 2 (Wail in 2A, Waleed in 2B), in the firstclass cabin. They
boarded American 11 between 7:31 and 7:40. The aircraft pushed back from the gate at 7:40.

- ```grep -e "Banihammad" -e "Suqami" chapter-1.txt```
    Between 6:45 and 7:40, Atta and Omari, along with Satam al Suqami, Wail al Shehri, and Waleed al Shehri, checked in and boarded American  
Airlines Flight 11, bound for Los Angeles. The flight was scheduled to depart at 7:45.
    In another Logan terminal, Shehhi, joined by Fayez Banihammad, Mohand al Shehri, Ahmed al Ghamdi, and Hamza al Ghamdi, checked in for     
United Airlines Flight 175, also bound for Los Angeles. A couple of Shehhi's colleagues were obviously unused to travel; according to the     
United ticket agent, they had trouble understanding the standard security questions, and she had to go over them slowly until they gave the   
routine, reassuring answers.
    While Atta had been selected by CAPPS in Portland, three members of his hijacking team-Suqami, Wail al Shehri, and Waleed al Shehri-were  
selected in Boston. Their selection affected only the handling of their checked bags, not their screening at the checkpoint. All five men     
cleared the checkpoint and made their way to the gate for American 11. Atta, Omari, and Suqami took their seats in business class (seats 8D,  
8G, and 10B, respectively). The Shehri brothers had adjacent seats in row 2 (Wail in 2A, Waleed in 2B), in the firstclass cabin. They
boarded American 11 between 7:31 and 7:40. The aircraft pushed back from the gate at 7:40.
    Shehhi and his team, none of whom had been selected by CAPPS, boarded United 175 between 7:23 and 7:28 (Banihammad in 2A, Shehri in 2B,   
Shehhi in 6C, Hamza al Ghamdi in 9C, and Ahmed al Ghamdi in 9D). Their aircraft pushed back from the gate just before 8:00.
    At the same time or shortly thereafter, Atta-the only terrorist on board trained to fly a jet-would have moved to the cockpit from his    
business-class seat, possibly accompanied by Omari. As this was happening, passenger Daniel Lewin, who was seated in the row just behind      
Atta and Omari, was stabbed by one of the hijackers-probably Satam al Suqami, who was seated directly behind Lewin. Lewin had served four     
years as an officer in the Israeli military. He may have made an attempt to stop the hijackers in front of him, not realizing that another    
was sitting behind him.

This command is useful if you want to search for two potential lines of text in a certain file. 

4. grep -i "pattern" file.txt
- ```grep -i "CAPPS" chapter-1.txt```
- Output:
When he checked in for his flight to Boston, Atta was selected by a computerized prescreening system known as CAPPS (Computer Assisted    
Passenger Prescreening System), created to identify passengers who should be subject to special security measures. Under security rules in    
place at the time, the only consequence of Atta's selection by CAPPS was that his checked bags were held off the plane until it was
confirmed that he had boarded the aircraft. This did not hinder Atta's plans.
    While Atta had been selected by CAPPS in Portland, three members of his hijacking team-Suqami, Wail al Shehri, and Waleed al Shehri-were  
selected in Boston. Their selection affected only the handling of their checked bags, not their screening at the checkpoint. All five men 
cleared the checkpoint and made their way to the gate for American 11. Atta, Omari, and Suqami took their seats in business class (seats 8D,  
8G, and 10B, respectively). The Shehri brothers had adjacent seats in row 2 (Wail in 2A, Waleed in 2B), in the firstclass cabin. They
boarded American 11 between 7:31 and 7:40. The aircraft pushed back from the gate at 7:40.
    Shehhi and his team, none of whom had been selected by CAPPS, boarded United 175 between 7:23 and 7:28 (Banihammad in 2A, Shehri in 2B,   
Shehhi in 6C, Hamza al Ghamdi in 9C, and Ahmed al Ghamdi in 9D). Their aircraft pushed back from the gate just before 8:00.
    Hani Hanjour, Khalid al Mihdhar, and Majed Moqed were flagged by CAPPS. The Hazmi brothers were also selected for extra scrutiny by the   
airline's customer service representative at the check-in counter. He did so because one of the brothers did not have photo identification    
nor could he understand English, and because the agent found both of the passengers to be suspicious. The only consequence of their
selection was that their checked bags were held off the plane until it was confirmed that they had boarded the aircraft.
    When the local civil aviation security office of the Federal Aviation Administration (FAA) later investigated these security screening    
operations, the screeners recalled nothing out of the ordinary. They could not recall that any of the passengers they screened were CAPPS     
selectees. We asked a screening expert to review the videotape of the hand-wanding, and he found the quality of the screener's work to have   
been "marginal at best." The screener should have "resolved" what set off the alarm; and in the case of both Moqed and Hazmi, it was clear    
that he did not.
    Newark: United 93. Between 7:03 and 7:39, Saeed al Ghamdi, Ahmed al Nami, Ahmad al Haznawi, and Ziad Jarrah checked in at the United      
Airlines ticket counter for Flight 93, going to Los Angeles. Two checked bags; two did not. Haznawi was selected by CAPPS. His checked bag    
was screened for explosives and then loaded on the plane.
- ```grep -i "FAA" chapter-1.txt```
- Output (Ommitted part of output because too long):
At the suggestion of the Boston Center's military liaison, NEADS contacted the FAA's Washington Center to ask about American 11. In the   
course of the conversation, a Washington Center manager informed NEADS:"We're looking- we also lost American 77." The time was 9:34. This     
was the first notice to the military that American 77 was missing, and it had come by chance. If NEADS had not placed that call, the NEADS    
air defenders would have received no information whatsoever that the flight was even missing, although the FAA had been searching for it. No  
one at FAA headquarters ever asked for military assistance with American 77.
    At 9:36, the FAA's Boston Center called NEADS and relayed the discovery about an unidentified aircraft closing in on Washington:"Latest   
report. Aircraft VFR [visual flight rules] six miles southeast of the White House. . . . Six, southwest. Six, southwest of the White House,   
deviating away." This startling news prompted the mission crew commander at NEADS to take immediate control of the airspace to clear a        
flight path for the Langley fighters:"Okay, we're going to turn it . . . crank it up. . . . Run them to the White House." He then
discovered, to his surprise, that the Langley fighters were not headed north toward the Baltimore area as instructed, but east over the       
ocean." I don't care how many windows you break," he said." Damn it. . . . Okay. Push them back."
    The Langley fighters were heading east, not north, for three reasons. First, unlike a normal scramble order, this order did not include   
a distance to the target or the target's location. Second, a "generic" flight plan-prepared to get the aircraft airborne and out of local     
airspace quickly-incorrectly led the Langley fighters to believe they were ordered to fly due east (090) for 60 miles. Third, the lead pilot  
and local FAA controller incorrectly assumed the flight plan instruction to go "090 for 60" superseded the original scramble order.

This might be useful if you want to look for text that is case sensitive in a certain file. 


**Citation**
I used Geeksforgeeks and DigitalOcean, which I found through google, for all of these commands.
