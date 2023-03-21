## 문법
```java
@Entity
@Table(
    uniqueConstraints={
        @UniqueConstraint(
            name={"uniqueConstraint"}
            columnNames={"col1", "col2"}
        )
    }
)
@Data
public class Entity{
    @Column(name="col1")
    int col1;
    
    @Column(name="col2")
    int col2;
}
```