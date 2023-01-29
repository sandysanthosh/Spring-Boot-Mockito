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

#### Reference 2:

```



import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Before;
import org.junit.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

public class TestWebApp extends SpringBootHelloWorldTests {

	@Autowired
	private WebApplicationContext webApplicationContext;

	private MockMvc mockMvc;

	@Before
	public void setup() {
		mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
	}

	@Test
	public void testEmployee() throws Exception {
		mockMvc.perform(get("/employee")).andExpect(status().isOk())
				.andExpect(content().contentType("application/json;charset=UTF-8"))
				.andExpect(jsonPath("$.name").value("emp1")).andExpect(jsonPath("$.designation").value("manager"))
				.andExpect(jsonPath("$.empId").value("1")).andExpect(jsonPath("$.salary").value(3000));

	}

}


```

#### Junit Test case Mockito:


```

@RunWith(SpringRunner.class)
@WebMvcTest(EmployeeController.class)
public class EmployeeControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private EmployeeService employeeService;

    private final ObjectMapper objectMapper = new ObjectMapper();

    @Test
    public void findAll_ShouldReturnListOfEmployees() throws Exception {
        List<Employee> employees = Arrays.asList(
                new Employee(1L, "John Doe", "Developer"),
                new Employee(2L, "Jane Smith", "Manager"));
        when(employeeService.findAll()).thenReturn(employees);

        mockMvc.perform(get("/employees"))
                .andExpect(status().isOk())
                .andExpect(content().json(objectMapper.writeValueAsString(employees)));
    }

    @Test
    public void findById_ShouldReturnEmployee() throws Exception {
        Employee employee = new Employee(1L, "John Doe", "Developer");
        when(employeeService.findById(1L)).thenReturn(employee);

        mockMvc.perform(get("/employees/1"))
                .andExpect(status().isOk())
                .andExpect(content().json(objectMapper.writeValueAsString(employee)));
    }

    @Test
    public void save_ShouldReturnSavedEmployee() throws Exception {
        Employee employee = new Employee(1L, "John Doe", "Developer");
        when(employeeService.save(employee)).thenReturn(employee);

        mockMvc.perform(post("/employees")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(employee)))
                .andExpect(status().isOk())
                .andExpect(content().json(objectMapper.writeValueAsString(employee)));
    }

    @Test
    public void deleteById_ShouldDeleteEmployee() throws Exception {
        doNothing().when(employeeService).deleteById(1L);

        mockMvc.perform(delete("/employees/1"))
                .andExpect(status().isOk());

        verify(employeeService, times(1)).deleteById(1L);
    }

}

```


