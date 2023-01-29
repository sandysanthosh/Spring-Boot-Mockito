#### junit Test program:


```

package com.javatechie.spring.mockito.api.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import com.javatechie.spring.mockito.api.model.User;
import com.javatechie.spring.mockito.api.service.UserService;

@RestController
public class UserController {
	@Autowired
	private UserService service;

	@PostMapping(value = "/save")
	public User saveUser(@RequestBody User user) {
		return service.addUser(user);
	}

	@GetMapping("/getUsers")
	public List<User> findAllUsers() {
		return service.getUsers();
	}

	@GetMapping("/getUserByAddress/{address}")
	public List<User> findUserByAddress(@PathVariable String address) {
		return service.getUserbyAddress(address);
	}

	@DeleteMapping(value="/remove")
	public User removeUser(@RequestBody User user) {
		service.deleteUser(user);
		return user;
	}
}

```


### junit test case:

```
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;

import java.util.Arrays;
import java.util.List;

import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

import com.javatechie.spring.mockito.api.controller.UserController;
import com.javatechie.spring.mockito.api.model.User;
import com.javatechie.spring.mockito.api.service.UserService;
import com.fasterxml.jackson.databind.ObjectMapper;

public class UserControllerTest {
    @Mock
    private UserService service;
    @InjectMocks
    private UserController controller;
    private MockMvc mockMvc;

    public void setUp() {
        MockitoAnnotations.initMocks(this);
        mockMvc = MockMvcBuilders.standaloneSetup(controller).build();
    }

    @Test
    public void testSaveUser() throws Exception {
        User user = new User(1, "John", "NY");
        when(service.addUser(user)).thenReturn(user);
        mockMvc.perform(MockMvcRequestBuilders.post("/save").contentType(MediaType.APPLICATION_JSON)
                .content(asJsonString(user))).andExpect(status().isOk());
        verify(service).addUser(user);
    }

    @Test
    public void testFindAllUsers() throws Exception {
        User user1 = new User(1, "John", "NY");
        User user2 = new User(2, "Mike", "LA");
        List<User> users = Arrays.asList(user1, user2);
        when(service.getUsers()).thenReturn(users);
        mockMvc.perform(MockMvcRequestBuilders.get("/getUsers")).andExpect(status().isOk())
                .andExpect(content().json(asJsonString(users)));
        verify(service).getUsers();
    }

    @Test
    public void testFindUserByAddress() throws Exception {
        User user1 = new User(1, "John", "NY");
        User user2 = new User(2, "Mike", "NY");
        List<User> users = Arrays.asList(user1, user2);
        when(service.getUserbyAddress("NY")).thenReturn(users);
        mockMvc.perform(MockMvcRequestBuilders.get

}
}


```
