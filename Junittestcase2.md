

#### Test case 2 referer program:

```
import java.util.ArrayList;  
import java.util.List;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Service;  
import com.javatpoint.model.Books;  
import com.javatpoint.repository.BooksRepository;  
//defining the business logic  
@Service  
public class BooksService   
{  
@Autowired  
BooksRepository booksRepository;  
//getting all books record by using the method findaAll() of CrudRepository  
public List<Books> getAllBooks()   
{  
List<Books> books = new ArrayList<Books>();  
booksRepository.findAll().forEach(books1 -> books.add(books1));  
return books;  
}  
//getting a specific record by using the method findById() of CrudRepository  
public Books getBooksById(int id)   
{  
return booksRepository.findById(id).get();  
}  
//saving a specific record by using the method save() of CrudRepository  
public void saveOrUpdate(Books books)   
{  
booksRepository.save(books);  
}  
//deleting a specific record by using the method deleteById() of CrudRepository  
public void delete(int id)   
{  
booksRepository.deleteById(id);  
}  
//updating a record  
public void update(Books books, int bookid)   
{  
booksRepository.save(books);  
}  
}  


```


#### junit test case program:

```

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.javatpoint.model.Books;
import com.javatpoint.repository.BooksRepository;

@ExtendWith(MockitoExtension.class)
public class BooksServiceTest {

    @Mock
    private BooksRepository booksRepository;

    @InjectMocks
    private BooksService booksService;

    private Books book1;
    private Books book2;
    private List<Books> books;

    @BeforeEach
    public void setUp() {
        book1 = new Books(1, "book1", "author1", "publisher1");
        book2 = new Books(2, "book2", "author2", "publisher2");
        books = Arrays.asList(book1, book2);
    }

    @Test
    public void testGetAllBooks() {
        when(booksRepository.findAll()).thenReturn(books);
        assertEquals(books, booksService.getAllBooks());
    }

    @Test
    public void testGetBooksById() {
        when(booksRepository.findById(1)).thenReturn(Optional.of(book1));
        assertEquals(book1, booksService.getBooksById(1));
    }

    @Test
    public void testSaveOrUpdate() {
        booksService.saveOrUpdate(book1);
    }

    @Test
    public void testDelete() {
        booksService.delete(1);
    }

    @Test
    public void testUpdate() {
        booksService.update(book1, 1);
    }
}


```


###### This test class uses the @Mock annotation to create a mock object for the BooksRepository. The @InjectMocks annotation is used to create an instance of the BooksService and inject the mock repository into it.

###### The test methods use the when() method of the mock repository to specify the behavior of the findAll(), findById(), and save() methods. The test methods then call the corresponding methods of the service and use assertEquals() to check that the service returns the expected results.

###### It is also important to note that this is a basic junit test case, in real world scenario you'll have to test all the possible edge cases, and test the methods under different scenarios.

