#### Junit Test case:


```

@RestController
@RequestMapping("/employees")
public class EmployeeController {
    private final EmployeeService employeeService;

    public EmployeeController(EmployeeService employeeService) {
        this.employeeService = employeeService;
    }

    @GetMapping
    public List<Employee> findAll() {
        return employeeService.findAll();
    }

    @GetMapping("/{id}")
    public Employee findById(@PathVariable Long id) {
        return employeeService.findById(id);
    }

    @PostMapping
    public Employee save(@RequestBody Employee employee) {
        return employeeService.save(employee);
    }

    @DeleteMapping("/{id}")
    public void deleteById(@PathVariable Long id) {
        employeeService.deleteById(id);
    }
}



```


### Junit Test case:


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

