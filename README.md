##Notes
I prepared seeder, so you can run php artisan migrate --seed before testing the project.

##Assumptions
1) Agent can call to any customers, not only assigned to him. If we need to show only assigned calls, we can easily add another condition in controller.
2) Anybody can call to anybody, and in the future we can add another entities exept agents and customers.
3) Duration of call is a number of seconds.

##Staps taken
1) Created agents and customers tables. Created table agent-customers to create many-to many relation between agents and customers (though, it is not used for table display now).
2) Created 'callables' tabel (id, callable_id, callable_type) and interface with trait for customers and agents. Each agent and each customer has an id in 'callables'.
3) Created 'call' table. 'Caller_id' and 'called_id' is ids from callables.
4) Created controller for table with index method. Placed html in welcome.blade.
5) For chosing period of time added datepicker.

##Bonus - speed up the loading
1) We can get rid of the table 'callables', and in 'call' table add agent_id, customer_id instead of caller_id and called_id, and the direction of call (if customers can also call). Table loading will be faster, BUT it will close this table for extensions (we will not be able to add another entities).
2) We can add index by agent name, if we need to search by agent name often. BUT it will slow down adding new agent.
3) We can replace eloquent methods with almost raw sql reauest like DB::raw("select .. join... limit... offset..."). This will be faster, because now we take 5 sql request to database, and DB::raw will do only one. BUT this code maybe will be hard to maintain in the future. Also, if we decide create raw sql request, we can hide part of the request into view. 

##TO DO
1) I would add CallTableResource to simplify the outcoming data, but I've stucked a little bit with how blade works with resource, as I usually do not use blades, but I'm 100% sure it could be done.
2) I would also replace get request with post request as it seems safer for filtering. Moreover, CallTableRequest should also be created to validate user's input.
