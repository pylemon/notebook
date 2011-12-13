
1. named a *.py* filename *memcache.py*

once you install this module, it will not work right. and it will
return *'module' object has no attribute 'Client'*



2. modify the url in urls.py but forget to modify the function in views
 
return *'module' object has no attribute 'taker_admin'*


3. make choices in models
   
	   GENDER_CHOICES = (
		   ('M', 'Male'),
		   ('F', 'Female'),
	   )
	   class Person(models.Model):
		   name = models.CharField(max_length=20)
		   gender = models.CharField(max_length=1, choices=GENDER_CHOICES)
	   
	   >>> p.gender
	   'M'
	   >>> p.get_gender_display()
	   'Male'

in template it show work like this:

	{{ person.get_gender_display }}

the mistake was:

	*{{ person.gender.get_gender_display }}*
