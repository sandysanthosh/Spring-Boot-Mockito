# spring-boot-mockito

How to write Unit test case using mockito framework 



#### Spring Boot:

* Controller
* Service
* Respository

```

@Runwith(SpringRunner.class)

@SpringBootTest
public class SpringBootMockitoApplicationTests{

@Autowired
private userservice service;

@MockBean
private UserRepository repository;

@Test
public void getUsersTest(){
when(repository.findAll()).thenReturn(Stream
.of(new User(376,"Danile",31,"USA"),
   (new User(958,"Huy",31,"UK")).collect(Collectors.toList()));
assertEquals(2, service.getUsers.size());
}
  
@Test
public void getUsersAddressTest(){
String address="Bangalore";
when(repository.findByAddress(address)).thenReturn(Stream
.of(new User(376,"Danile",31,"USA").collect(Collectors.toList()));
assertEquals(1, service.getUsersByaddress(address).size());
}

@Test
public void saveUserTest()
{
User user = new User(999,"Pranya",33,"Pune");
when(repository.save(user)).thenReturn(user);
asserEquals(user, Service.addUser(user);
}
}

@Test
Public void deleteUser()
{
User user = new User(999,"Pranya",33,"Pune");
service.deleteUser(user);
verify(repository,times(1)).delete(user);
}

}
  
  
  ```
