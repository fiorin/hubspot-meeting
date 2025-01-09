.: My understanding on the project
There are some gaps in the way Hubspot handles the associations and for this business logic lacks some ways to retrieve exactly what is needed in a decent way, so the proccess keeps the data sync with additional fields in the register, such as a foreigh key in a SQL database JOIN. Seems a smart workaround to keep using a stablished platform adapted to the product needs.

.: What I added in code
I followed strictly the instructions to follow the other methods implementation and added a 'processMeetings' for retrieving and updating the meetings, and a 'getAtendees' method to retrieve the contacts associated to a meeting, because the meeting list endpoint does not return all the meeting details.
I kept the methods along the other ones, for the 'standards', but I will discuss the improves in the next lines.

.: Issues I have found
I wrote the getAttendes using the v3 method to fetch association from a meeting and its attending contacts. 1:N association. But didn't brought results for the meetings. Not sure if the entity names: meeting; contact; are wrong in some way. (Tested plural, and variantions) or if there are no associations in the DB. However, that represents my logic behind how to retrieve the contacts and to update the attendees in the meeting object as an extra field.

.: Improves

.: (1) code quality and readbility
It is hard to follow the processing in a single worker.js file. All the process method should be moved to service alike file, and the internal blocks turned in methods to reduce complexity and have a clear view of each single method goal. Shorter multiple files.

.: (2) project architecture
I didn't liked the procedural thinking to execute the worker, overriding multiple times non-needed register. The main process: contacts; companies; and meetings; should be moved to non dependent commands. So if you want to execute independently, that will be possible. Could have a combined as well, but essentially reducing sequential dependency.
The log system should be supported by a permanent file log with all the critical occurences.

.: (3) code performance
In my implementation I did a loop fetching contacts for each meeting. Maybe a way to combine meeting IDs and fetching a single time for multiple meetings would reduce execution time and request failure chances.
Another point for each process, avoiding overriding the data for each register every time. That can causa data loss, because there are no rollback. So, updating ONLY the mutated ones would be nicer. That reduce the execution loop and keep the non mutated data safer.
