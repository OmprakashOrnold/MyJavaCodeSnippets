# CRUD Operations Using Core java concepts

```java
package com.crud.collections;

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class Employee {

	private int emp_no;
	private String emp_name;
	private int emp_salary;

	public Employee(int emp_no, String emp_name, int emp_salary) {
		super();
		this.emp_no = emp_no;
		this.emp_name = emp_name;
		this.emp_salary = emp_salary;
	}

	public int getEmp_no() {
		return emp_no;
	}

	public void setEmp_no(int emp_no) {
		this.emp_no = emp_no;
	}

	public String getEmp_name() {
		return emp_name;
	}

	public void setEmp_name(String emp_name) {
		this.emp_name = emp_name;
	}

	public int getEmp_salary() {
		return emp_salary;
	}

	public void setEmp_salary(int emp_salary) {
		this.emp_salary = emp_salary;
	}

	@Override
	public String toString() {
		return "Employee [emp_no=" + emp_no + ", emp_name=" + emp_name + ", emp_salary=" + emp_salary + "]";
	}

}

class RegisterRepo {
	Map<Integer, Employee> employees = new HashMap<Integer, Employee>();

	public String saveEmployee(Employee employee) {
		employees.put(employee.getEmp_no(), employee);
		return "Employee Added Successfully...";
	}

	public String updateStudent(Employee employee) {
		int empNo = employee.getEmp_no();
		if (employees.containsKey(empNo)) {
			employees.put(employee.getEmp_no(), employee);
			return "Employee Updated Successfully...";
		} else {
			return "Employee not available with this roll No.";
		}
	}

	public Map<Integer, Employee> getEmployees() {
		return employees;
	}

	public String deleteStudentByRollNo(int empNo) {
		employees.remove(empNo);
		return "Employee Removed Successfully";
	}

}

public class CRUDUsingCollections {

	public static void main(String[] args) {
		RegisterRepo registerRepo = new RegisterRepo();

		Scanner scanner = new Scanner(System.in);
		String answer = "n";
		int option = 0;
		System.out.println("Employee Registration...");
		do {
			System.out.println("Select operation \n1. Display Employee List\n"
					+ "2. Add Employee\n3. Update Employee\n4. Delete Employee");
			option = scanner.nextInt();
			switch (option) {
			case 1:
				// List<Student> students = registerRepo.getStudents();
				Map<Integer, Employee> employees = registerRepo.getEmployees();
				for (Map.Entry<Integer, Employee> hm : employees.entrySet()) {
					System.out.println("Employee: " + hm.getValue());
				}
				break;
			case 2:
				System.out.println("Enter Employee No. :");
				int empNo = scanner.nextInt();
				System.out.println("Enter Name: ");
				String name = scanner.next();
				System.out.println("Enter Salary: ");
				int salary = scanner.nextInt();
				Employee employee = new Employee(empNo, name, salary);
				String msg = registerRepo.saveEmployee(employee);
				System.out.println(msg);
				break;
			case 3:
				System.out.println("Enter Employee No. to Update :");
				empNo = scanner.nextInt();
				System.out.println("Enter Name to Update : ");
				name = scanner.next();
				System.out.println("Enter salary to Update : ");
				salary = scanner.nextInt();
				employee = new Employee(empNo, name, salary);
				msg = registerRepo.updateStudent(employee);
				System.out.println(msg);
				break;
			case 4:
				System.out.println("Enter Employee No. to Delete Student :");
				empNo = scanner.nextInt();
				msg = registerRepo.deleteStudentByRollNo(empNo);
				System.out.println(msg);
				break;
			default:
			}
			System.out.println("Do you want to continue..(y/n)");
			answer = scanner.next();
			if (answer.equals("n")) {
				System.out.println("Thank You");
				System.exit(0);
			}

		} while (option <= 5);
	}

}


```
