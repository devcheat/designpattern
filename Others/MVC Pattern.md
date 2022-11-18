**MVC Pattern**
===

MVC Pattern stands for Model-View-Controller Pattern. This pattern is used to separate application's concerns.

- **Model** - Model represents an object or JAVA POJO carrying data. It can also have logic to update controller if its data changes.

- **View** - View represents the visualization of the data that model contains.

- **Controller** - Controller acts on both model and view. It controls the data flow into model object and updates the view whenever data changes. It keeps view and model separate.

## Implementation
We are going to create a Student object acting as a model.StudentView will be a view class which can print student details on console and StudentController is the controller class responsible to store data in Student object and update view StudentView accordingly.

 
### Step 1: Create Model. `Student.cs`

```cs
public class Student {
   private String rollNo;
   private String name;
   
   public String getRollNo() {
      return rollNo;
   }
   
   public void setRollNo(String rollNo) {
      this.rollNo = rollNo;
   }
   
   public String getName() {
      return name;
   }
   
   public void setName(String name) {
      this.name = name;
   }
}
```

### Step 2: Create View. `StudentView.cs`

```cs
public class StudentView {
   public void printStudentDetails(String studentName, String studentRollNo){
      Console.WriteLine("Student: ");
      Console.WriteLine("Name: " + studentName);
      Console.WriteLine("Roll No: " + studentRollNo);
   }
}
```

### Step 3: Create Controller. `StudentController.cs`

```cs
public class StudentController {
   private Student model;
   private StudentView view;

   public StudentController(Student model, StudentView view){
      this.model = model;
      this.view = view;
   }

   public void setStudentName(String name){
      model.setName(name);		
   }

   public String getStudentName(){
      return model.getName();		
   }

   public void setStudentRollNo(String rollNo){
      model.setRollNo(rollNo);		
   }

   public String getStudentRollNo(){
      return model.getRollNo();		
   }

   public void updateView(){				
      view.printStudentDetails(model.getName(), model.getRollNo());
   }	
}
```

### Step 4: `MVCPatternDemo.cs`
Use the StudentController methods to demonstrate MVC design pattern usage.

```cs

public class MVCPatternDemo {
   public static void main(String[] args) {

      //fetch student record based on his roll no from the database
      Student model  = retriveStudentFromDatabase();

      //Create a view : to write student details on console
      StudentView view = new StudentView();

      StudentController controller = new StudentController(model, view);

      controller.updateView();

      //update model data
      controller.setStudentName("John");

      controller.updateView();
   }

   private static Student retriveStudentFromDatabase(){
      Student student = new Student();
      student.setName("Robert");
      student.setRollNo("10");
      return student;
   }
}
```
