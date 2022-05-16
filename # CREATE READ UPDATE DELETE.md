# CREATE READ UPDATE DELETE



-- c (create)
insert into address(name, handphone, address) values('개발자1', '010-1111-1111', '대한민국');
insert into address(name, handphone, address) values('개발자2', '010-2222-2222', '일본');
insert into address(name, handphone, address) values('개발자3', '010-3333-3333', '러시아');

-- r (read)
select addressnum , name, handphone , address from address;

select addressnum , name , handphone, address 
from address 
where addressnum = 1;

select addressnum, name , handphone, address 
from address 
order by name desc;

-- u (update)
update address
	set handphone = '010-5555-5555',
    address = '터키'
    where addressnum = 1;
    
    -- D(delete)
    delete from address
    where addressnum = 1;
    
  0
