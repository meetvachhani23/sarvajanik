# sarvajanik

if firstName != '' then
		 conditional := 'where first_name = '''||firstName||'''';
	end if;

	if lastName != '' then 
		if conditional != '' then
			conditional := conditional||' and last_name = '''||lastName||'''';
		else 
			conditional := 'where last_name = '''||lastName||'''';
		end if;
	end if;
	if gender != '' then 
		if conditional != '' then
			conditional := conditional||' and sex = '''||gender||'''';
		else 
			conditional := 'where sex = '''||gender||'''';
		end if;
	end if;
	if dateofbirth != '' then 
		if conditional != '' then
			conditional := conditional||' and dob = '''||dateofbirth||'''';
		else 
			conditional := 'where dob = '''||dateofbirth||'''';
		end if;
	end if;
  
  query1 := query1||conditional||' ORDER BY '|| orderby|| ' ASC LIMIT '|| pageSize ||' OFFSET (('||pageNumber||'-1) *'|| pageSize||')';
	raise notice 'sql %' , query1;
	return query execute query1;
