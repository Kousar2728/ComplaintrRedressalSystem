# ComplaintrRedressalSystem

package com.abctelecom.configuration;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
@Configuration
public class MyConfig implements WebMvcConfigurer {
@Override
public void addCorsMappings(CorsRegistry registry) {
registry.addMapping("/users/**")
.allowedOrigins
.allowedMethods("GET", "POST", "PUT", "DELETE");
}
}
package com.abctelecom.controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import com.abctelecom.dto.UserDto;
import com.abctelecom.entity.User;
import com.abctelecom.service.UserService;
@RestController
@CrossOrigin
@RequestMapping("/login")
public class LoginController {
@Autowired
private UserService userService;
@PostMapping("/check")
public User loginHandler(@RequestBody UserDto userDto) {
System.out.println(userDto.getEmail());
System.out.println(userService.loginCheck(userDto).getEmail());
return userService.loginCheck(userDto);
}
}
package com.abctelecom.controller;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import com.abctelecom.entity.Feedback;
import com.abctelecom.entity.Notes;
import com.abctelecom.entity.Ticket;
import com.abctelecom.service.TicketService;
@RestController
@CrossOrigin
@RequestMapping("/tickets")
public class TicketController {
@Autowired
private TicketService ticketService;
public TicketController(TicketService ticketService) {
this.ticketService = ticketService;
}
@PostMapping("/new")
public String raiseNew(@RequestBody Ticket ticket) {
return ticketService.save(ticket);
}
@GetMapping("/")
public List<Ticket> getAllTickets() {
return ticketService.getAllTickets();
}
@GetMapping("/customer/{id}")
public List<Ticket> getTicketsByCustomerId(@PathVariable String id) {
return ticketService.getTicketsByCustomerId(Long.parseLong(id));
}
@GetMapping("/{id}")
public Ticket getTicketById(@PathVariable String id) {
return ticketService.getTicketById(Long.parseLong(id));
}
@GetMapping("/notes/{ticketId}")
public List<Notes> getNotesByTicketId(@PathVariable String ticketId) {
return ticketService.getNotesByTicketId(Long.parseLong(ticketId));
}
@PostMapping("/notes/save")
public String saveNotes(@RequestBody Notes notes) {
return ticketService.saveNotes(notes);
}
@GetMapping("/feedback/{ticketId}")
public Feedback getFeedbackByTicket(@PathVariable String ticketId) {
return ticketService.findFeedbackByTicket(Long.parseLong(ticketId));
}
@PostMapping("/feedback/submit")
public String submitFeedback(@RequestBody Feedback feedback) {
return ticketService.saveFeedback(feedback);
}
}
package com.abctelecom.controller;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import com.abctelecom.dto.CustomerDto;
import com.abctelecom.entity.Customer;
import com.abctelecom.entity.Engineer;
import com.abctelecom.entity.Manager;
import com.abctelecom.service.UserService;
@RestController
@CrossOrigin
@RequestMapping("/users")
public class UserController {
@Autowired
private UserService userService;
@GetMapping("/")
public ResponseEntity<String> apiTest(){
return new ResponseEntity<String>("Hi user", HttpStatus.OK);
}
@GetMapping("/zipcodes")
public List<String> getZipcodes(){
System.out.println(userService.getZipcodes());
return userService.getZipcodes();
}
@PostMapping("/customers/new")
public String createCustomer(@RequestBody CustomerDto customerDto) {
Customer customer = new Customer(customerDto);
return userService.addCustomer(customer);
}
@PostMapping("/engineers/new")
public String createEngineer(@RequestBody Engineer engineer) {
return userService.addEngineer(engineer);
}
@PostMapping("/managers/new")
public String createManager(@RequestBody Manager manager) {
return userService.addManager(manager);
}
@GetMapping("/customers")
public List<Customer> getAllCustomers(){
return userService.getAllCustomers();
}
@GetMapping("customers/{id}")
public Customer getCustomerById(@PathVariable String id) {
return userService.getCustomerById(Long.parseLong(id));
}
@GetMapping("customers/findByUserId/{id}")
public Customer findCustomerByUserId(@PathVariable String id) {
return userService.findCustomerByUserId(Long.parseLong(id));
}
@GetMapping("engineers/findByUserId/{id}")
public Engineer findEngineerByUserId(@PathVariable String id) {
return userService.findEngineerByUserId(Long.parseLong(id));
}
@GetMapping("managers/findByUserId/{id}")
public Manager findBYUserId(@PathVariable String id) {
return userService.findManagerByUserId(Long.parseLong(id));
}
@GetMapping("engineers/{id}")
public Engineer getEngineerById(@PathVariable String id) {
return userService.getEngineerById(Long.parseLong(id));
}
@GetMapping("managers/{id}")
public Manager getManagerById(@PathVariable String id) {
return userService.getManagerById(Long.parseLong(id));
}
@GetMapping("/engineers")
public List<Engineer> getAllEngineers(){
return userService.getAllEngineers();
}
@GetMapping("engineers/byZipcode/{zipcode}")
public List<Engineer> getEngineersByZipcode(@PathVariable String zipcode){
return userService.getEngineersByZipcode(zipcode);
}
@GetMapping("/managers")
public List<Manager> getAllManagers(){
return userService.getAllManagers();
}
@DeleteMapping("/customers/{id}")
public String deleteCustomer(@PathVariable String id) {
return userService.deleteCustomer(Long.parseLong(id));
}
}
package com.abctelecom.dto;
import java.util.Set;
import com.abctelecom.entity.User;
import lombok.Getter;
import lombok.Setter;
@Getter
@Setter
public class CustomerDto {
private Long id;
private User user;
private Set<String> serviceType;
private String address;
private String zipcode;
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public User getUser() {
return user;
}
public void setUser(User user) {
this.user = user;
}
public Set<String> getServiceType() {
return serviceType;
}
public void setServiceType(Set<String> serviceType) {
this.serviceType = serviceType;
}
public String getAddress() {
return address;
}
public void setAddress(String address) {
this.address = address;
}
public String getZipcode() {
return zipcode;
}
public void setZipcode(String zipcode) {
this.zipcode = zipcode;
}
}
package com.abctelecom.dto;
public class UserDto {
private String email;
private String password;
public String getEmail() {
return email;
}
public void setEmail(String email) {
this.email = email;
}
public String getPassword() {
return password;
}
public void setPassword(String password) {
this.password = password;
}
}
package com.abctelecom.entity;
import java.util.Set;
import com.abctelecom.dto.CustomerDto;
import jakarta.persistence.CascadeType;
import jakarta.persistence.Column;
import jakarta.persistence.ElementCollection;
import jakarta.persistence.Entity;
import jakarta.persistence.FetchType;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinTable;
import jakarta.persistence.OneToOne;
import jakarta.persistence.Table;
import jakarta.persistence.*;
@Entity
@Table
public class Customer {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@OneToOne(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
private User user;
@ElementCollection(fetch = FetchType.EAGER)
@JoinTable(name = "customer_service", joinColumns = @JoinColumn(name = "customer_id"))
private Set<String> serviceType;
@Column
private String address;
@Column
private String zipcode;
public Customer(Long id, User user, Set<String> serviceType, String address, String zipcode)
{
super();
this.id = id;
this.user = user;
this.serviceType = serviceType;
this.address = address;
this.zipcode = zipcode;
}
public Customer(CustomerDto customerDto) {
this.user = customerDto.getUser();
this.serviceType = customerDto.getServiceType();
this.address = customerDto.getAddress();
this.zipcode = customerDto.getZipcode();
}
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public User getUser() {
return user;
}
public void setUser(User user) {
this.user = user;
}
public Set<String> getServiceType() {
return serviceType;
}
public void setServiceType(Set<String> serviceType) {
this.serviceType = serviceType;
}
public String getAddress() {
return address;
}
public void setAddress(String address) {
this.address = address;
}
public String getZipcode() {
return zipcode;
}
public void setZipcode(String zipcode) {
this.zipcode = zipcode;
}
}
package com.abctelecom.entity;
import java.time.LocalDate;
import org.hibernate.annotations.CreationTimestamp;
import jakarta.persistence.Entity;
import jakarta.persistence.*;
@Entity
@Table
public class Engineer {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@OneToOne(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
private User user;
@CreationTimestamp
private LocalDate createdOn;
@Column
private String zipcode;
public Engineer() {
super();
// TODO Auto-generated constructor stub
}
public Engineer(Long id, User user, LocalDate createdOn, String zipcode) {
super();
this.id = id;
this.user = user;
this.createdOn = createdOn;
this.zipcode = zipcode;
}
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public User getUser() {
return user;
}
public void setUser(User user) {
this.user = user;
}
public LocalDate getCreatedOn() {
return createdOn;
}
public void setCreatedOn(LocalDate createdOn) {
this.createdOn = createdOn;
}
public String getZipcode() {
return zipcode;
}
public void setZipcode(String zipcode) {
this.zipcode = zipcode;
}
}
package com.abctelecom.entity;
import java.time.LocalDate;
import org.hibernate.annotations.CreationTimestamp;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.OneToOne;
import jakarta.persistence.Table;
@Entity
@Table
public class Feedback {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@Column
private Integer rating;
@Column
private String comments;
@OneToOne
private Ticket ticket;
@CreationTimestamp
private LocalDate createdOn;
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public Integer getRating() {
return rating;
}
public void setRating(Integer rating) {
this.rating = rating;
}
public String getComments() {
return comments;
}
public void setComments(String comments) {
this.comments = comments;
}
public Ticket getTicket() {
return ticket;
}
public void setTicket(Ticket ticket) {
this.ticket = ticket;
}
public LocalDate getCreatedOn() {
return createdOn;
}
public void setCreatedOn(LocalDate createdOn) {
this.createdOn = createdOn;
}
}
package com.abctelecom.entity;
import java.time.LocalDate;
import java.util.Set;
import org.hibernate.annotations.CreationTimestamp;
import jakarta.persistence.*;
@Entity
@Table
public class Manager {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@OneToOne(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
private User user;
@CreationTimestamp
private LocalDate createdOn;
@ElementCollection
@JoinTable(name = "manager_pincode",joinColumns = @JoinColumn(name = "manager_id"))
private Set<String> zipcode;
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public User getUser() {
return user;
}
public void setUser(User user) {
this.user = user;
}
public LocalDate getCreatedOn() {
return createdOn;
}
public void setCreatedOn(LocalDate createdOn) {
this.createdOn = createdOn;
}
public Set<String> getZipcode() {
return zipcode;
}
public void setZipcode(Set<String> zipcode) {
this.zipcode = zipcode;
}
}
package com.abctelecom.entity;
import java.time.LocalDate;
import org.hibernate.annotations.CreationTimestamp;
import jakarta.persistence.*;
@Entity
@Table
public class Notes {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@Column
private String comments;
@ManyToOne
private Ticket ticket;
@CreationTimestamp
private LocalDate createdOn;
@Column
private boolean isCustomer;
// updated by USER/ENGINEER's Name
@Column
private String updatedBy;
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public String getComments() {
return comments;
}
public void setComments(String comments) {
this.comments = comments;
}
public Ticket getTicket() {
return ticket;
}
public void setTicket(Ticket ticket) {
this.ticket = ticket;
}
public LocalDate getCreatedOn() {
return createdOn;
}
public void setCreatedOn(LocalDate createdOn) {
this.createdOn = createdOn;
}
public boolean isCustomer() {
return isCustomer;
}
public void setCustomer(boolean isCustomer) {
this.isCustomer = isCustomer;
}
public String getUpdatedBy() {
return updatedBy;
}
public void setUpdatedBy(String updatedBy) {
this.updatedBy = updatedBy;
}
}
package com.abctelecom.entity;
public enum ServiceType {
LANDLINE, MOBILE, FIBER_OPTIC;
}
package com.abctelecom.entity;
public enum Status {
RAISED, ASSIGNED, WIP, RESOLVED, ESCALATED;
}
package com.abctelecom.entity;
import java.time.LocalDate;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;
import jakarta.persistence.*;
@Entity
@Table
public class Ticket {
@TableGenerator(name = "Ticket_Gen", initialValue = 100000, allocationSize = 1)
@Id
@GeneratedValue(strategy = GenerationType.TABLE, generator = "Ticket_Gen")
private Long id;
@Column
private String serviceType;
@Column
private String issueType;
@CreationTimestamp
private LocalDate createdOn;
@UpdateTimestamp
private LocalDate updatedOn;
@Column
private String description;
@ManyToOne
private Customer customer;
@Column
private String address;
@Column
private String zipcode;
@Column
private String mobile;
// @Enumerated(EnumType.STRING)
@Column
private String status;
@ManyToOne
private Engineer engineer;
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public String getServiceType() {
return serviceType;
}
public void setServiceType(String serviceType) {
this.serviceType = serviceType;
}
public String getIssueType() {
return issueType;
}
public void setIssueType(String issueType) {
this.issueType = issueType;
}
public LocalDate getCreatedOn() {
return createdOn;
}
public void setCreatedOn(LocalDate createdOn) {
this.createdOn = createdOn;
}
public LocalDate getUpdatedOn() {
return updatedOn;
}
public void setUpdatedOn(LocalDate updatedOn) {
this.updatedOn = updatedOn;
}
public String getDescription() {
return description;
}
public void setDescription(String description) {
this.description = description;
}
public Customer getCustomer() {
return customer;
}
public void setCustomer(Customer customer) {
this.customer = customer;
}
public String getAddress() {
return address;
}
public void setAddress(String address) {
this.address = address;
}
public String getZipcode() {
return zipcode;
}
public void setZipcode(String zipcode) {
this.zipcode = zipcode;
}
public String getMobile() {
return mobile;
}
public void setMobile(String mobile) {
this.mobile = mobile;
}
public String getStatus() {
return status;
}
public void setStatus(String status) {
this.status = status;
}
public Engineer getEngineer() {
return engineer;
}
public void setEngineer(Engineer engineer) {
this.engineer = engineer;
}
}
package com.abctelecom.entity;
import jakarta.persistence.*;
@Entity
@Table
public class User {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
@Column
private String fullName;
@Column
private String role;
@Column
private String email;
@Column
private String phone;
@Column
private String password;
public User(Long id, String fullName, String role, String email, String phone, String
password) {
super();
this.id = id;
this.fullName = fullName;
this.role = role;
this.email = email;
this.phone = phone;
this.password = password;
}
public User() {
super();
// TODO Auto-generated constructor stub
}
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public String getFullName() {
return fullName;
}
public void setFullName(String fullName) {
this.fullName = fullName;
}
public String getRole() {
return role;
}
public void setRole(String role) {
this.role = role;
}
public String getEmail() {
return email;
}
public void setEmail(String email) {
this.email = email;
}
public String getPhone() {
return phone;
}
public void setPhone(String phone) {
this.phone = phone;
}
public String getPassword() {
return password;
}
public void setPassword(String password) {
this.password = password;
}
}
package com.abctelecom.entity;
import jakarta.persistence.*;
@Entity
@Table(name = "zipcodes")
public class Zipcode {
@Id
private Long id;
@Column
private String zipcode;
public Long getId() {
return id;
}
public void setId(Long id) {
this.id = id;
}
public String getZipcode() {
return zipcode;
}
public void setZipcode(String zipcode) {
this.zipcode = zipcode;
}
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import com.abctelecom.entity.Customer;
import com.abctelecom.entity.User;
public interface CustomerRepository extends JpaRepository<Customer, Long> {
List<Customer> findByUser(User user);
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import com.abctelecom.entity.Engineer;
import com.abctelecom.entity.User;
public interface EngineerRepository extends JpaRepository<Engineer, Long>{
List<Engineer> findByUser(User user);
List<Engineer> findByZipcode(String zipcode);
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import com.abctelecom.entity.Feedback;
import com.abctelecom.entity.Ticket;
public interface FeedbackRepository extends JpaRepository<Feedback, Long> {
List<Feedback> findByTicket(Ticket ticket);
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import com.abctelecom.entity.Manager;
import com.abctelecom.entity.User;
public interface ManagerRepository extends JpaRepository<Manager, Long>{
List<Manager> findByUser(User user);
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.web.bind.annotation.CrossOrigin;
import com.abctelecom.entity.Notes;
import com.abctelecom.entity.Ticket;
@Repository
public interface NotesRepository extends JpaRepository<Notes, Long> {
List<Notes> findByTicket(Ticket ticket);
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.web.bind.annotation.CrossOrigin;
import com.abctelecom.entity.Customer;
import com.abctelecom.entity.Engineer;
import com.abctelecom.entity.Ticket;
@CrossOrigin
public interface TicketRepository extends JpaRepository<Ticket, Long> {
List<Ticket> findByCustomer(Customer customer);
List<Ticket> findByZipcode(String zipcode);
List<Ticket> findByEngineer(Engineer engineer);
}
package com.abctelecom.repository;
import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import com.abctelecom.entity.User;
public interface UserRepository extends JpaRepository<User, Long>{
List<User> findByEmail(String email);
}
package com.abctelecom.repository;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.abctelecom.entity.Zipcode;
@Repository
public interface ZipcodeRepository extends JpaRepository<Zipcode, Long> {
}
package com.abctelecom.service;
import java.util.List;
//import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.abctelecom.entity.Customer;
import com.abctelecom.entity.Feedback;
import com.abctelecom.entity.Notes;
import com.abctelecom.entity.Ticket;
import com.abctelecom.repository.FeedbackRepository;
import com.abctelecom.repository.NotesRepository;
import com.abctelecom.repository.TicketRepository;
@Service
public class TicketServiceImpl implements TicketService {
private TicketRepository ticketRepository;
private NotesRepository notesRepository;
private FeedbackRepository feedbackRepository;
public TicketServiceImpl(TicketRepository ticketRepository, NotesRepository notesRepository,
FeedbackRepository feedbackRepository) {
this.feedbackRepository = feedbackRepository;
this.ticketRepository = ticketRepository;
this.notesRepository = notesRepository;
}
@Override
public String save(Ticket ticket) {
if (ticket.getStatus() == null) {
ticket.setStatus("RAISED");
}
return ticketRepository.save(ticket).getId().toString();
}
@Override
public List<Ticket> getAllTickets() {
return ticketRepository.findAll();
}
@Override
public List<Ticket> getTicketsByCustomerId(Long id) {
Customer customer = new Customer(null);
customer.setId(id);
return ticketRepository.findByCustomer(customer);
}
@Override
public Ticket getTicketById(Long id) {
return ticketRepository.findById(id).get();
}
@Override
public List<Notes> getNotesByTicketId(Long id) {
Ticket ticket = ticketRepository.findById(id).get();
return notesRepository.findByTicket(ticket);
}
@Override
public String saveNotes(Notes notes) {
return notesRepository.save(notes).getTicket().getId().toString();
}
@Override
public String saveFeedback(Feedback feedback) {
return feedbackRepository.save(feedback).getTicket().getId().toString();
}
@Override
public Feedback findFeedbackByTicket(Long id) {
Ticket ticket = ticketRepository.findById(id).get();
return feedbackRepository.findByTicket(ticket).get(0);
}
}
package com.abctelecom.service;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.abctelecom.dto.UserDto;
import com.abctelecom.entity.Customer;
import com.abctelecom.entity.Engineer;
import com.abctelecom.entity.Manager;
import com.abctelecom.entity.User;
import com.abctelecom.repository.CustomerRepository;
import com.abctelecom.repository.EngineerRepository;
import com.abctelecom.repository.ManagerRepository;
import com.abctelecom.repository.UserRepository;
import com.abctelecom.repository.ZipcodeRepository;
@Service
public class UserServiceImpl implements UserService {
@Autowired
private CustomerRepository customerRepository;
@Autowired
private EngineerRepository engineerRepository;
@Autowired
private ManagerRepository managerRepository;
@Autowired
private ZipcodeRepository zipcodeRepository;
@Autowired
private UserRepository userRepository;
// Password encoder will be added after adding the security dependency
// private BCryptPasswordEncoder passwordEncoder;
@Override
public List<String> getZipcodes(){
List<String> zipcodes = new ArrayList<String>();
zipcodeRepository.findAll().stream().forEach(zipcode->zipcodes.add(zipcode.getZipcode()));
return zipcodes;
}
@Override
public String addCustomer(Customer customer) {
return customerRepository.save(customer).getId().toString();
}
@Override
public String addEngineer(Engineer engineer) {
return engineerRepository.save(engineer).getId().toString();
}
@Override
public String addManager(Manager manager) {
return managerRepository.save(manager).getId().toString();
}
@Override
public List<Customer> getAllCustomers() {
return customerRepository.findAll();
}
@Override
public List<Engineer> getAllEngineers() {
return engineerRepository.findAll();
}
@Override
public List<Manager> getAllManagers() {
return managerRepository.findAll();
}
@Override
public Customer getCustomerById(long id) {
Optional<Customer> optional= customerRepository.findById(id);
if(optional.isPresent())
return optional.get();
return new Customer(null);
}
@Override
public Engineer getEngineerById(long id) {
Optional<Engineer> optional = engineerRepository.findById(id);
return optional.get();
}
@Override
public Manager getManagerById(long id) {
return managerRepository.findById(id).get();
}
@Override
public String deleteCustomer(Long id) {
customerRepository.delete(getCustomerById(id));
return id.toString();
}
@Override
public User loginCheck(UserDto userDto) {
User user;
List<User> users = userRepository.findByEmail(userDto.getEmail());
if(!users.isEmpty()) {
user = users.get(0);
if(user.getPassword().equals(userDto.getPassword())) {
return user;
}
}
return null;
}
@Override
public Customer findCustomerByUserId(Long id) {
User user = userRepository.findById(id).get();
return customerRepository.findByUser(user).get(0);
}
@Override
public Engineer findEngineerByUserId(Long id) {
User user = userRepository.findById(id).get();
return engineerRepository.findByUser(user).get(0);
}
@Override
public Manager findManagerByUserId(Long id) {
User user = userRepository.findById(id).get();
return managerRepository.findByUser(user).get(0);
}
@Override
public List<Engineer> getEngineersByZipcode(String zipcode) {
return engineerRepository.findByZipcode(zipcode);
}
}
server.port=8081
spring.datasource.url = jdbc:mysql://localhost:3306/abctelecom
spring.datasource.username = root
spring.datasource.password = Redhat@1234.
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQLDialect
spring.jpa.generate-ddl=true
spring.jpa.properties.hibernate.format_sql=true
logging.level.org.hibernate.type=trace
spring.data.rest.base-path=/api
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
https://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>3.1.4</version>
<relativePath/> <!-- lookup parent from repository -->
</parent>
<groupId>com.abctelecom</groupId>
<artifactId>abcTelecom</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>abcTelecom</name>
<description>Project for complaint redressal system</description>
<properties>
<java.version>17</java.version>
</properties>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-devtools</artifactId>
<scope>runtime</scope>
<optional>true</optional>
</dependency>
<dependency>
<groupId>com.mysql</groupId>
<artifactId>mysql-connector-j</artifactId>
<scope>runtime</scope>
</dependency>
<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
<scope>provided</scope>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
<scope>test</scope>
</dependency>
</dependencies>
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>
</project>
FRONTEND:
import { User } from "./user";
export class Customer {
id: number;
user: User;
serviceType: string[];
address: string;
zipcode: string;
}
import { User } from "./user";
export class Engineer {
id: number;
user: User;
zipcode: string;
}
import { Ticket } from "./ticket";
export class Feedback {
id: number;
rating: number;
comments: string;
ticket: Ticket;
}
import { User } from "./user";
export class Manager {
id: number;
user: User;
zipcode: string[];
}
export class Multiselect {
item_id: number;
item_text: string;
group: string;
}
import { Ticket } from "./ticket";
export class Notes {
id: number;
ticket: Ticket;
comments: string;
updatedBy: string;
createdOn: Date;
isCustomer: boolean;
}
import { Customer } from "./customer";
import { Engineer } from "./engineer";
export class Ticket {
id: number;
address: string
customer: Customer;
engineer: Engineer;
description: string;
issueType: string;
mobile: string;
serviceType: string;
zipcode: string;
createdOn: Date;
updatedOn: Date;
status: string;
}
export class User {
id: number;
fullName: string;
role: string;
email: string;
phone: string;
password: string;
}
export class Zipcode {
zipcode: string;
}
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable, Subject } from 'rxjs';
import { User } from '../classes/user';
import { Customer } from '../classes/customer';
import { Engineer } from '../classes/engineer';
import { Manager } from '../classes/manager';
import { HttpClient } from '@angular/common/http'
import { UserService } from './user.service';
import { Userdto } from '../classes/userdto';
@Injectable({
providedIn: 'root'
})
export class LoginService {
// loggedInUser:Subject<User>=new BehaviorSubject<User>(null);
loggedInUser: Subject<User | null> = new BehaviorSubject<User | null>(null);
loggedInCustomer: Subject<Customer | null> = new BehaviorSubject<Customer | null>(null);
loggedInEngineer: Subject<Engineer | null> = new BehaviorSubject<Engineer | null>(null);
loggedInManager: Subject<Manager | null> = new BehaviorSubject<Manager | null>(null);
constructor(private http: HttpClient, private userService: UserService) { }
checkLogin(userDto: Userdto): Observable<User> {
const loginUrl = "http://localhost:8081/login/check";
this.http.post<User>(loginUrl, userDto).subscribe(data => {
this.loggedInUser.next(data);
// If user is a customer then get their details
if (data.role == 'customer') {
this.userService.getCustomerByUserId(data.id).subscribe(data => {
this.loggedInCustomer.next(data);
})
}
// If user is an Engineer then get their details
if (data.role == 'engineer') {
this.userService.getEngineerByUserId(data.id).subscribe(data => {
this.loggedInEngineer.next(data);
})
}
// If user is an Manager then get their details
if (data.role == 'manager') {
this.userService.getManagerByUserId(data.id).subscribe(data => {
this.loggedInManager.next(data);
})
}
})
return this.http.post<User>(loginUrl, userDto);
}
}
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { Ticket } from '../classes/ticket';
import { Engineer } from '../classes/engineer';
import { Notes } from '../classes/notes';
import { Feedback } from '../classes/feedback';
@Injectable({
providedIn: 'root'
})
export class TicketService {
private postUrl: string = 'http://localhost:8081/tickets/new';
constructor(private httpClient: HttpClient) { }
raiseNewTicket(ticket: Ticket): Observable<any> {
return this.httpClient.post<Ticket>(this.postUrl, ticket);
}
getCustomerTickets(customerId: number): Observable<Ticket[]> {
const getUrl = "http://localhost:8081/tickets/customer/" + customerId;
return this.httpClient.get<Ticket[]>(getUrl);
}
getAllTickets(): Observable<Ticket[]> {
const getUrl = "http://localhost:8081/tickets/";
return this.httpClient.get<Ticket[]>(getUrl);
}
getTicketById(id: number): Observable<Ticket> {
const getUrl = "http://localhost:8081/tickets/" + id;
return this.httpClient.get<Ticket>(getUrl);
}
getEngineersByZipcode(zipcode: string): Observable<Engineer[]> {
const getUrl = 'http://localhost:8081/users/engineers/byZipcode/' + zipcode;
return this.httpClient.get<Engineer[]>(getUrl);
}
getNotesByTicketId(id: number): Observable<Notes[]> {
const getUrl = 'http://localhost:8081/tickets/notes/' + id;
return this.httpClient.get<Notes[]>(getUrl);
}
saveNotes(notes: Notes): Observable<any> {
const postUrl = 'http://localhost:8081/tickets/notes/save';
return this.httpClient.post<Notes>(postUrl, notes);
}
saveFeedback(feedBack: Feedback): Observable<any> {
const postUrl = 'http://localhost:8081/tickets/feedback/submit';
return this.httpClient.post<Feedback>(postUrl, feedBack);
}
getFeedback(ticketId: number): Observable<Feedback> {
const getUrl = 'http://localhost:8081/tickets/feedback/' + ticketId;
return this.httpClient.get<Feedback>(getUrl);
}
}
import { Injectable } from '@angular/core';
import { Customer } from '../classes/customer';
import { Engineer } from '../classes/engineer';
import { Manager } from '../classes/manager';
import { HttpClient } from '@angular/common/http';
import { Observable, map } from 'rxjs';
@Injectable({
providedIn: 'root'
})
export class UserService {
loggedInCustomer!: Customer;
loggedInEngineer!: Engineer;
loggedInManager!: Manager;
constructor(private http: HttpClient) { }
createManager(manager: Manager): Observable<any> {
const postUrl = 'http://localhost:8081/users/managers/new';
return this.http.post<Manager>(postUrl, manager);
}
createEngineer(engineer: Engineer): Observable<any> {
const postUrl = 'http://localhost:8081/users/engineers/new';
return this.http.post<Engineer>(postUrl, engineer);
}
createCustomer(customer: Customer): Observable<any> {
const postUrl = 'http://localhost:8081/users/customers/new';
// customer.ticket=null;
return this.http.post<Customer>(postUrl, customer);
}
getZipcodes(): Observable<string[]> {
const getUrl = "http://localhost:8081/users/zipcodes/";
console.log(this.http.get(getUrl));
return this.http.get<string[]>(getUrl).pipe(
map((response: any) => response)
)
}
getCustomers(): Observable<Customer[]> {
const getUrl = 'http://localhost:8081/users/customers';
return this.http.get<Customer[]>(getUrl);
}
getCustomerById(id: number): Observable<Customer> {
const getUrl = 'http://localhost:8081/users/customers/' + id;
return this.http.get<Customer>(getUrl);
}
getCustomerByUserId(id: number): Observable<Customer> {
const getUrl = "http://localhost:8081/users/customers/findByUserId/" + id;
return this.http.get<Customer>(getUrl);
}
getEngineerByUserId(id: number): Observable<Engineer> {
const getUrl = "http://localhost:8081/users/engineers/findByUserId/" + id;
return this.http.get<Engineer>(getUrl);
}
getManagerByUserId(id: number): Observable<Manager> {
const getUrl = "http://localhost:8081/users/managers/findByUserId/" + id;
return this.http.get<Manager>(getUrl);
}
getEngineers(): Observable<Engineer[]> {
const getUrl = 'http://localhost:8081/users/engineers';
return this.http.get<Engineer[]>(getUrl);
}
getEngineerById(id: number): Observable<Engineer> {
const getUrl = 'http://localhost:8081/users/engineers/' + id;
return this.http.get<Engineer>(getUrl);
}
getManagers(): Observable<Manager[]> {
const getUrl = 'http://localhost:8081/users/managers';
return this.http.get<Manager[]>(getUrl);
}
getManagerById(id: number): Observable<Manager> {
const getUrl = 'http://localhost:8081/users/managers/' + id;
return this.http.get<Manager>(getUrl);
}
deleteCustomer(id: number): Observable<any> {
const deleteUrl = 'http://localhost:8081/users/customers/' + id;
return this.http.delete(deleteUrl);
}
}
<app-core></app-core>
<router-outlet></router-outlet>
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { LoginComponent } from './components/login/login.component';
import { AssignenginnerComponent } from './components/assignenginner/assignenginner.component';
import { CoreComponent } from './components/core/core.component';
import { CreateCustomerComponent } from
'./components/create-customer/create-customer.component';
import { CreateEmployeeComponent } from
'./components/create-employee/create-employee.component';
import { CreateUserComponent } from './components/create-user/create-user.component';
import { FeedbackComponent } from './components/feedback/feedback.component';
import { HomeComponent } from './components/home/home.component';
import { TicketComponent } from './components/ticket/ticket.component';
import { TicketCustomerComponent } from
'./components/ticket-customer/ticket-customer.component';
import { TicketEditComponent } from './components/ticket-edit/ticket-edit.component';
import { TicketListComponent } from './components/ticket-list/ticket-list.component';
import { UserListComponent } from './components/user-list/user-list.component';
import { ReactiveFormsModule } from '@angular/forms';
import { RouterModule, Routes } from '@angular/router';
import { HttpClientModule } from '@angular/common/http';
import { NgMultiSelectDropDownModule } from 'ng-multiselect-dropdown';
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
const routes: Routes = [
{ path: 'home', component: TicketListComponent },
{ path: "ticket", component: TicketComponent },
{ path: "ticket-list", component: TicketListComponent },
{ path: "create", component: CreateUserComponent },
{ path: "users/:role", component: UserListComponent },
{ path: "editUser/:id/:role", component: CreateUserComponent },
{ path: "login", component: LoginComponent },
{ path: "viewTicket/:id", component: TicketComponent },
{ path: "feedback/:ticketId", component: FeedbackComponent },
{ path: '**', redirectTo: '/login', pathMatch: 'full' }
];
@NgModule({
declarations: [
AppComponent,
LoginComponent,
AssignenginnerComponent,
CoreComponent,
CreateCustomerComponent,
CreateEmployeeComponent,
CreateUserComponent,
FeedbackComponent,
HomeComponent,
TicketComponent,
TicketCustomerComponent,
TicketEditComponent,
TicketListComponent,
UserListComponent
],
imports: [
BrowserModule,
AppRoutingModule,
ReactiveFormsModule,
RouterModule.forRoot(routes),
HttpClientModule,
NgMultiSelectDropDownModule.forRoot(),
NgbModule
],
providers: [],
bootstrap: [AppComponent]
})
export class AppModule { }
<!-- <p>core works!</p> -->
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
<div class="container-fluid ms-4 me-4">
<a href="#" class="navbar-brand">ABC Telecom Ltd.</a>
<button type="button" class="navbar-toggler" data-bs-toggle="collapse"
data-bs-target="#navbarCollapse1">
<span class="navbar-toggler-icon"></span>
</button>
<div class="collapse navbar-collapse" id="navbarCollapse1">
<div class="navbar-nav">
<a routerLink="/home" class="nav-item nav-link"
routerLinkActive="active-link">Home</a>
<!-- Check if loggedInUser exists -->
<ng-container *ngIf="loggedInUser">
<!-- Dropdown for Tickets -->
<li class="nav-item dropdown">
<a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#"
role="button"
aria-expanded="false">Tickets</a>
<ul class="dropdown-menu">
<li><a class="dropdown-item" [routerLink]="['/ticket-list']">All
Tickets</a></li>
<ng-container *ngIf="loggedInUser?.role === 'customer'">
<li>
<hr class="dropdown-divider">
</li>
<li><a class="dropdown-item" routerLink="/ticket">Create
New</a></li>
</ng-container>
</ul>
</li>
<!-- Dropdown for Users if not a customer -->
<ng-container *ngIf="loggedInUser?.role !== 'customer'">
<li class="nav-item dropdown">
<a class="nav-link dropdown-toggle" data-bs-toggle="dropdown"
href="#" role="button"
aria-expanded="false">Users</a>
<ul class="dropdown-menu">
<li><a class="dropdown-item"
[routerLink]="['/users','customer']">Customers</a></li>
<li><a class="dropdown-item"
[routerLink]="['/users','engineer']">Engineers</a></li>
<li><a class="dropdown-item"
[routerLink]="['/users','manager']">Managers</a></li>
<ng-container *ngIf="loggedInUser?.role === 'admin'">
<li>
<hr class="dropdown-divider">
</li>
<li><a class="dropdown-item" routerLink="/create">New
User</a></li>
</ng-container>
</ul>
</li>
</ng-container>
<!-- Logout Link -->
<a routerLink="/logout" class="nav-item nav-link">Logout</a>
<!-- Display User Name -->
<div class="profile-icon">
<h4 class="profile-name">{{loggedInUser?.fullName}}</h4>
<!-- SVG code can be placed here -->
</div>
</ng-container>
</div>
</div>
</div>
</nav>
import { Component } from '@angular/core';
import { Router } from '@angular/router';
import { User } from 'src/app/classes/user';
import { LoginService } from 'src/app/services/login.service';
import { UserService } from 'src/app/services/user.service';
@Component({
selector: 'app-core',
templateUrl: './core.component.html',
styleUrls: ['./core.component.css']
})
export class CoreComponent {
loggedInUser: any;
setUserToNull() {
this.loggedInUser = null;
}
constructor(
private userService: UserService,
private loginService: LoginService,
private router: Router
) { }
ngOnInit(): void {
if (this.loginService.loggedInUser != null) {
this.loginService.loggedInUser.subscribe(data => {
this.loggedInUser != data;
})
}
else {
this.router.navigateByUrl('login');
}
}
}
<!-- <p>create-user works!</p> -->
<br>
<div class="col-md-5 m-auto">
<div class="card shadow-lg p-3 border-3">
<div class="card-header">
<h3 class="">{{formHeading}}</h3>
</div>
<div class="card-body">
<form [formGroup]="userForm" (ngSubmit)="submitHandle()" class="row g-3">
<div formGroupName="user">
<input type="hidden" formControlName='id'>
<div class="form-group">
<label class="control-lable" for="fullName"> Full Name* </label>
<input id="fullName" formControlName="fullName" class="form-control">
</div>
<div class="form-group">
<label class="control-lable" for="phone"> phone* </label>
<input id="phone" formControlName="phone" class="form-control">
</div>
<div class="form-group">
<label class="control-lable" for="email"> email* </label>
<input id="email" formControlName="email" class="form-control">
</div>
<div class="form-group">
<label class="control-lable" for="password"> password* </label>
<input type="password" id="password" formControlName="password"
class="form-control">
</div>
<div class="form-group">
<label for="role" class="control-label"> Role* </label>
<select id="role" formControlName="role" (change)="roleHandler()"
class="form-select">
<option value="customer">Customer</option>
<option value="engineer">Engineer</option>
<option value="manager">Manager</option>
</select>
</div>
</div>
<!-- Form for customer -->
<!-- *************** -->
<div *ngIf="role=='customer'">
<div formGroupName="customer">
<input type="hidden" formControlName='id'>
<div class="form-group">
<label class="control-lable" for="address"> Address* </label>
<input id="address" formControlName="address" class="form-control">
</div>
<div class="form-group">
<label class="control-lable" for="zipcode"> Zipcode* </label>
<select formControlName='zipcode' id="zipcode" class="form-select">
<option *ngFor="let zipcode of zipcodes"
[ngValue]="zipcode">{{zipcode}}</option>
</select>
</div>
<div class="form-group">
<label class="control-lable" for="address"> Service Type* </label>
<ng-multiselect-dropdown formControlName="serviceType"
[placeholder]="'service type'"
[settings]="dropdownSettings" [data]="serviceList"
(onSelect)="onItemSelect($event)"
(onSelectAll)="onSelectAll($event)"> </ng-multiselect-dropdown>
</div>
</div>
</div>
<!-- Form for engineer -->
<!-- *************** -->
<div *ngIf="role=='engineer'">
<div formGroupName="engineer">
<input type="hidden" formControlName='id'>
<div class="form-group">
<label class="control-lable" for="zipcode"> Zipcode* </label>
<select formControlName='zipcode' id="zipcode" class="form-select">
<option *ngFor="let zipcode of zipcodes"
[ngValue]="zipcode">{{zipcode}}</option>
</select>
</div>
</div>
</div>
<!-- Form for Manager -->
<!-- *************** -->
<div *ngIf="role=='manager'">
<div formGroupName="manager">
<input type="hidden" formControlName='id'>
<div class="form-group">
<label class="control-lable"> Zipcodes Assigned* </label>
<ng-multiselect-dropdown formControlName="zipcode"
[placeholder]="'zipcode'"
[settings]="dropdownSettings" [data]="zipcodeList"
(onSelect)="onItemSelect($event)"
(onSelectAll)="onSelectAll($event)"> </ng-multiselect-dropdown>
</div>
</div>
</div>
<div class="mt-3">
<button class="btn btn-dark" type="submit">Submit</button>&nbsp;
<button type="reset" class="btn btn-dark">Cancel</button>
</div>
</form>
</div>
</div>
</div>
import { Component } from '@angular/core';
import { AbstractControl, FormBuilder, FormControl, FormGroup, Validators } from
'@angular/forms';
import { ActivatedRoute } from '@angular/router';
import { Observable, map } from 'rxjs';
import { Customer } from 'src/app/classes/customer';
import { Engineer } from 'src/app/classes/engineer';
import { Manager } from 'src/app/classes/manager';
import { Multiselect } from 'src/app/classes/multiselect';
import { User } from 'src/app/classes/user';
import { UserService } from 'src/app/services/user.service';
@Component({
selector: 'app-create-user',
templateUrl: './create-user.component.html',
styleUrls: ['./create-user.component.css']
})
export class CreateUserComponent {
userForm: FormGroup;
formHeading: string = 'New user';
role: string;
zipcodes: string[] = [];
constructor(
private formBuilder: FormBuilder,
private userService: UserService,
private route: ActivatedRoute
) { }
serviceList: any;
zipcodeList: any;
dropdownSettings: any;
userId: number = 0;
userRole: string = '';
ngOnInit(): void {
this.serviceList = this.getData();
this.zipcodeList = this.getZipcodes();
this.dropdownSettings = {
singleSelection: false,
idField: 'item_id',
textField: 'item_text',
selectAllText: 'Select All',
unSelectAllText: 'UnSelect All'
};
this.userForm = this.formBuilder.group({
user: this.formBuilder.group({
id: [''],
fullName: new FormControl("", [Validators.required]),
role: new FormControl("", [Validators.required]),
email: new FormControl("", [Validators.required, Validators.email]),
phone: new FormControl("", [Validators.required]),
password: new FormControl("", [Validators.required])
}),
customer: this.formBuilder.group({
id: [''],
serviceType: new FormControl("", [Validators.required]),
address: new FormControl("", [Validators.required]),
zipcode: new FormControl("", [Validators.required]),
}),
engineer: this.formBuilder.group({
id: [''],
zipcode: new FormControl("", [Validators.required])
}),
manager: this.formBuilder.group({
id: [''],
zipcode: new FormControl("", [Validators.required])
})
});
this.route.paramMap.subscribe(() => {
this.editUser();
})
}
editUser() {
if (this.route.snapshot.paramMap.has('id')) {
this.formHeading = 'Edit User';
this.userId = +this.route.snapshot.paramMap.get('id')!;
this.userRole != this.route.snapshot.paramMap.get('role');
let editUserForm = this.userForm.get('user');
// If role is Customer then set Customer form
if (this.userRole == 'customer') {
this.editCustomer(editUserForm!);
}
if (this.userRole == 'engineer') {
this.editEngineer(editUserForm!);
}
if (this.userRole == 'manager') {
this.editManager(editUserForm!);
}
}
}
editManager(editUserForm: AbstractControl) {
let editManagerForm = this.userForm.get('manager');
this.userService.getManagerById(this.userId).subscribe(data => {
editUserForm.setValue(data.user);
this.roleHandler();
editManagerForm!.get('zipcode')!.setValue(data.zipcode);
editManagerForm!.get('id')!.setValue(data.id);
})
}
editEngineer(editUserForm: AbstractControl) {
let editEngineerForm = this.userForm.get('engineer');
this.userService.getEngineerById(this.userId).subscribe(data => {
editUserForm.setValue(data.user);
this.roleHandler();
editEngineerForm!.get('zipcode')!.setValue(data.zipcode);
editEngineerForm!.get('id')!.setValue(data.id);
})
}
editCustomer(editUserForm: AbstractControl) {
let editCustomerForm = this.userForm.get('customer')
this.userService.getCustomerById(this.userId).subscribe(data => {
console.log(data);
editUserForm.setValue(data.user);
this.roleHandler();
editCustomerForm!.get('address')!.setValue(data.address);
editCustomerForm!.get('serviceType')!.setValue(data.serviceType);
editCustomerForm!.get('zipcode')!.setValue(data.zipcode);
editCustomerForm!.get('id')!.setValue(data.id);
/**Instead of the below data assigned userform as data.user
* editUserForm.get('fullName').setValue(data.user.fullName);
editUserForm.get('role').setValue(data.user.role);
editUserForm.get('phone').setValue(data.user.phone);
editUserForm.get('email').setValue(data.user.email);
editUserForm.get('password').setValue(data.user.password);
*/
this.formHeading = 'Edit User'
})
}
getZipcodes(): Observable<Multiselect[]> {
return this.userService.getZipcodes().pipe(
map((data: string[]) => {
return data.map((zipcode, index) => {
return {
item_id: index,
item_text: zipcode,
group: 'z'
} as Multiselect;
});
})
);
}
// getZipcodes(): Array<Multiselect> {
// let multiSelect: Multiselect[] = [];
// this.userService.getZipcodes().subscribe((data: string[]) => {
// this.zipcodes = data;
// console.log(this.zipcodes);
// for (let i = 0; i < data.length; i++) {
// let theMultiSelect = new Multiselect();
// theMultiSelect.item_id = i;
// theMultiSelect.item_text = data[i];
// theMultiSelect.group = 'z';
// multiSelect.push(theMultiSelect);
// }
// })
// return multiSelect;
// }
getData(): Array<Multiselect> {
return [
{ item_id: 1, item_text: 'landline', group: 'S' },
{ item_id: 2, item_text: 'phone', group: 'S' },
{ item_id: 3, item_text: 'fiber optic', group: 'S' },
];
}
onItemSelect(item: any) {
// console.log(item);
}
onSelectAll(items: any) {
// console.log(items);
}
roleHandler() {
this.role = this.userForm.get('user')!.value.role;
// console.log("==========>Role Selected: "+this.role);
}
submitHandle() {
// console.log(this.userForm.get('user').value);
// console.log(this.userForm.get('customer').value);
let user: User = this.userForm.get('user')!.value;
// For Customer Role
// ********************
if (this.role == 'customer') {
const services: Multiselect[] = this.userForm.get('customer')!.value.serviceType;
let serviceToString: string[] = [];
services.map(data => serviceToString.push(data.item_text));
// serviceToString.map(data=>console.log(data));
let customer: Customer = new Customer();
customer = this.userForm.get('customer')!.value;
customer.serviceType = serviceToString;
customer.user = user;
this.userService.createCustomer(customer).subscribe({
next: response => {
alert(`Customer created successfully! Customer ID: ${response}`);
},
error: err => {
alert(`There was an error: ${err.message}`);
}
})
}
// For Engineer Role
// ********************
if (this.role == 'engineer') {
let engineer: Engineer = new Engineer();
engineer = this.userForm.get('engineer')!.value;
engineer.user = user;
this.userService.createEngineer(engineer).subscribe({
next: response => {
alert(`Engineer created successfully! Engineer ID: ${response}`);
},
error: err => {
alert(`There was an error: ${err.message}`);
}
})
}
// For Manager Role
// ********************
if (this.role == "manager") {
let manager: Manager = new Manager();
const pincodes: Multiselect[] = this.userForm.get('manager')!.value.zipcode;
let pincodesToString: string[] = [];
pincodes.map(data => pincodesToString.push(data.item_text));
manager = this.userForm.get('manager')!.value;
manager.user = user;
manager.zipcode = pincodesToString;
this.userService.createManager(manager).subscribe({
next: response => {
alert(`Manager created successfully! Manager ID: ${response}`);
},
error: err => {
alert(`There was an error: ${err.message}`);
}
})
}
}
}
<p>feedback works!</p>
<div class="container">
<form [formGroup]="feedbackForm" (ngSubmit)="submitHandler()">
<div class="row justify-content-around">
<div class="col-sm-3">
<h4>Feedback - Ticket# {{ticket.id}} </h4>
<div class="form-group">
<label for="rating" class="control-label">
Rating
</label>
<select id="rating" class="form-select" formControlName="rating">
<option value="1">1</option>
<option value="2">2</option>
<option value="3">3</option>
<option value="4">4</option>
<option value="5">5</option>
</select>
</div>
</div>
</div>
<div class="row justify-content-around">
<div class="col-sm-3">
<div class="form-group">
<label class="control-lable" for="comments">
Comments
</label>
<textarea id="comments" formControlName="comments" class="form-control"
cols="30"
rows="5"></textarea>
</div>
</div>
</div>
<br>
<div class="row justify-content-around">
<div class="col-sm-3">
<button class="btn btn-dark">Submit</button>
</div>
</div>
<br>
<div class="row justify-content-around">
<div class="col-sm-3">
<!-- <button class="btn btn-dark">Cancel</button> -->
</div>
</div>
</form>
</div>
import { Component } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';
import { ActivatedRoute } from '@angular/router';
import { Feedback } from 'src/app/classes/feedback';
import { Ticket } from 'src/app/classes/ticket';
import { TicketService } from 'src/app/services/ticket.service';
@Component({
selector: 'app-feedback',
templateUrl: './feedback.component.html',
styleUrls: ['./feedback.component.css']
})
export class FeedbackComponent {
ticket!: Ticket;
feedbackForm!: FormGroup;
constructor(
private ticketService: TicketService,
private router: ActivatedRoute,
private formBuilder: FormBuilder
) { }
ngOnInit(): void {
if (this.router.snapshot.paramMap.has('ticketId'))
this.ticketService.getTicketById(+this.router.snapshot.paramMap.get('ticketId')!).subscribe(data
=> {
this.ticket = data;
});
this.feedbackForm = this.formBuilder.group({
id: [''],
rating: [''],
comments: ['']
});
}
submitHandler() {
let feedback: Feedback = new Feedback();
feedback = this.feedbackForm.value;
feedback.ticket = this.ticket;
console.log(feedback);
this.ticketService.saveFeedback(feedback).subscribe({
next: res => {
alert("Feedback submitted successfully" + res);
},
error: err => {
alert("There was an error" + err.message);
}
})
}
}
<p>home works!</p>
<div class="container" style="margin: 5px 5px;">
<h3 style="margin:auto;width: 300px;">Create New User</h3>
<div class="row">
<div class="col md-3">
<button class="btn btn-success">New Customer</button>
</div>
<div class="col md-3">
<button class="btn btn-success">New Employee</button>
</div>
<div class="col md-3">
<button class="btn btn-success">New Manager</button>
</div>
</div>
</div>
<div class="container">
<div class="col-md-8 m-auto mt-5">
<div class="card-group">
<div class="card rounded-0" style="height: 23rem;">
<div class="card-body">
<img src="assets/images/login.jpg" class="card-img-top rounded-0" alt="..."
style="height: 21rem;">
</div>
</div>
<div class="card rounded-0">
<div class="card-body">
<h3 class="fw-bold m-3 text-uppercase" style="color: #393f81;">ABC Telecom
Ltd.</h3>
<p class="text-dark-50 mb-3 ">Please enter your email and password! </p>
<form [formGroup]="loginForm" (ngSubmit)="loginhandler()">
<div class="form-outline form-white mb-3">
<input type="email" formControlName='email' id="typeEmailX"
placeholder='Enter Email'
class="form-control" />
</div>
<div class="form-outline form-white mb-4">
<input formControlName='password' type="password" id="typePasswordX"
placeholder='Enter Password' class="form-control" />
</div>
<button class="btn btn-dark px-5" type="submit">Login</button>
<div>
<p class="mt-3 pb-lg-2" style="color: #393f81;">Don't have an
account?
<a routerLink="/create" style="color: #393f81;">Register
here</a>
</p>
</div>
</form>
</div>
</div>
</div>
</div>
</div>
import { Component } from '@angular/core';
import { FormGroup, FormBuilder, FormControl, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { Userdto } from 'src/app/classes/userdto';
import { LoginService } from 'src/app/services/login.service';
import { UserService } from 'src/app/services/user.service';
@Component({
selector: 'app-login',
templateUrl: './login.component.html',
styleUrls: ['./login.component.css']
})
export class LoginComponent {
loginForm: FormGroup;
constructor(
private formBulder: FormBuilder,
private loginService: LoginService,
private userService: UserService,
private router: Router
) { }
ngOnInit(): void {
this.loginForm = this.formBulder.group({
email: new FormControl('', [Validators.required, Validators.email]),
password: new FormControl('', Validators.required)
});
}
loginhandler() {
let userDto: Userdto = this.loginForm.value;
this.loginService.checkLogin(userDto).subscribe({
next: response => {
if (response != null) {
alert(`Logged in successfully! User Name: ${response.fullName}`);
if (response.role == 'customer') {
this.userService.getCustomerByUserId(response.id).subscribe(data => {
this.userService.loggedInCustomer = data;
this.router.navigateByUrl("ticket-list");
})
}
else if (response.role == 'engineer') {
this.userService.getEngineerByUserId(response.id).subscribe(data => {
this.userService.loggedInEngineer = data;
this.router.navigateByUrl("ticket-list");
})
}
else if (response.role == 'manager') {
this.userService.getManagerByUserId(response.id).subscribe(data => {
this.userService.loggedInManager = data;
this.router.navigateByUrl("ticket-list");
})
}
else if (response.role == 'admin') {
this.userService.loggedInCustomer;
this.loginService.loggedInUser.subscribe(data => {
console.log(data);
this.router.navigateByUrl("home");
})
}
else {
alert("something went wrong");
}
}
},
error: err => {
// alert(`There was an error: ${err.message}`);
alert("incorrect credentials")
}
})
}
}
import { Component } from '@angular/core';
import { FormGroup, FormBuilder, FormControl, Validators } from '@angular/forms';
import { ActivatedRoute } from '@angular/router';
import { Customer } from 'src/app/classes/customer';
import { Engineer } from 'src/app/classes/engineer';
import { Feedback } from 'src/app/classes/feedback';
import { Manager } from 'src/app/classes/manager';
import { Notes } from 'src/app/classes/notes';
import { Ticket } from 'src/app/classes/ticket';
import { User } from 'src/app/classes/user';
import { LoginService } from 'src/app/services/login.service';
import { TicketService } from 'src/app/services/ticket.service';
@Component({
selector: 'app-ticket',
templateUrl: './ticket.component.html',
styleUrls: ['./ticket.component.css']
})
export class TicketComponent {
complaintForm!: FormGroup;
loggedInUser!: User;
loggedInCustomer: Customer = new Customer();
loggedInEngineer!: Engineer;
loggedInmanagre!: Manager;
serviceType: string = '';
assignedTo: string = '';
ticket!: Ticket;
allEngineers!: Engineer[];
engineers!: Engineer[];
notes!: Notes[];
lastkeydown1: number = 0;
subscription: any;
feedback!: Feedback;
constructor(
private formBuilder: FormBuilder,
private ticketService: TicketService,
private loginService: LoginService,
private route: ActivatedRoute
) { }
ngOnInit(): void {
this.complaintForm = this.formBuilder.group({
ticket: this.formBuilder.group({
id: [''],
mobile: [''],
serviceType: [''],
issueType: [''],
description: [''],
zipcode: [''],
address: [''],
createdOn: [''],
updatedOn: [''],
status: [''],
// notes:[''],
customer: this.formBuilder.group({
id: [''],
serviceType: new FormControl("", [Validators.required]),
address: new FormControl("", [Validators.required]),
zipcode: new FormControl("", [Validators.required]),
user: this.formBuilder.group({
id: [''],
fullName: new FormControl("", [Validators.required]),
role: new FormControl("", [Validators.required]),
email: new FormControl("", [Validators.required, Validators.email]),
phone: new FormControl("", [Validators.required]),
password: new FormControl("", [Validators.required])
})
}),
// engineer:['']
engineer: this.formBuilder.group({
id: [''],
zipcode: new FormControl("", [Validators.required]),
user: this.formBuilder.group({
id: [''],
fullName: new FormControl("", [Validators.required]),
role: new FormControl("", [Validators.required]),
email: new FormControl("", [Validators.required, Validators.email]),
phone: new FormControl("", [Validators.required]),
password: new FormControl("", [Validators.required])
})
}),
}),
notes: this.formBuilder.group({
id: [''],
ticket: [''],
comments: [''],
createdOn: [''],
updatedBy: [''],
isCustomer: ['']
})
});
this.loginService.loggedInUser.subscribe(data => {
this.loggedInUser != data;
if (this.route.snapshot.paramMap.has('id')) {
this.route.paramMap.subscribe(() => {
this.getTicketDetails();
})
}
else if (this.loggedInUser.role = "customer") {
console.log("line 103");
this.getTicketDetailsByCustomer();
}
});
}
// End ngOnInit
getTicketDetails() {
// "+" prefixed here will cast string to a number
this.ticketService.getTicketById(+this.route.snapshot.paramMap.get('id')!).subscribe(data =>
{
this.ticket = data;
this.ticketService.getFeedback(data.id).subscribe(data => {
this.feedback = data;
})
this.ticketService.getNotesByTicketId(data.id).subscribe(data => {
this.notes = data;
});
console.log(this.ticket.serviceType + " at line 121");
this.serviceType = this.ticket.serviceType;
if (this.ticket.engineer != null) {
this.assignedTo = this.ticket.engineer.user.email;
this.complaintForm.get('ticket')!.get('engineer')!.patchValue(this.ticket.engineer);
console.log("At line 136===>" + this.assignedTo);
}
this.complaintForm.get('ticket')!.patchValue({
id: this.ticket.id,
mobile: this.ticket.mobile,
serviceType: this.ticket.serviceType,
issueType: this.ticket['issueType'],
description: this.ticket.description,
zipcode: this.ticket.zipcode,
address: this.ticket.address,
createdOn: this.ticket.createdOn,
updatedOn: this.ticket.updatedOn,
status: this.ticket.status,
customer: this.ticket.customer
});
this.complaintForm.get('ticket')!.disable();
if (this.loggedInUser.role == 'engineer') {
this.loginService.loggedInEngineer.subscribe(data => {
this.loggedInEngineer != data;
if (this.ticket.engineer.id == data!.id) {
this.complaintForm.get('ticket')!.get('status')!.enable({ onlySelf: true });
}
});
}
else if (this.loggedInUser.role == 'manager') {
console.log("line 153");
this.loginService.loggedInManager.subscribe(data => {
this.loggedInmanagre != data;
console.log("line 157");
this.complaintForm.get('ticket')!.get('engineer')!.enable({ onlySelf: true });
console.log("Enabled Engineer");
});
// populate Engineers for auto complete textbox
this.ticketService.getEngineersByZipcode(this.ticket.zipcode).subscribe(data => {
this.allEngineers = data;
this.engineers = data;
})
}
else if (this.loggedInUser.role == 'customer') {
this.loginService.loggedInCustomer.subscribe(data => this.loggedInCustomer != data);
}
console.log(this.ticket.serviceType + " at line 125");
// this.complaintForm.get('ticket').get('serviceType').setValue(ticket.serviceType);
})
}
getTicketDetailsByCustomer() {
console.log("line 110");
this.loginService.loggedInUser.subscribe(data => {
console.log("line 113");
this.loggedInUser != data;
let ticketFrom = this.complaintForm.get('ticket');
this.loginService.loggedInCustomer.subscribe(data => {
this.loggedInCustomer != data;
// assign all the customer details to ticket in order to auto fill the form
console.log("line 119");
ticketFrom!.get('customer')!.get('user')!.get("fullName")!.setValue(data!.user.fullName);
ticketFrom!.get('customer')!.get('user')!.get("fullName")!.disable();
ticketFrom!.get('mobile')!.setValue(data!.user.phone);
ticketFrom!.get('address')!.setValue(data!.address);
ticketFrom!.get('zipcode')!.setValue(data!.zipcode);
})
})
}
// Auto complete list function
getAutoComplList() {
let engineerEmail =
this.complaintForm!.get('ticket')!.get('engineer')!.get('user')!.get('email')!.value;
// this.engineers = [];
// if (engineerEmail.length > 2) {
// if ($event.timeStamp+200> 200) {
// this.engineers = this.searchFromArray(this.allEngineers, engineerEmail);
// }
// }
console.log(engineerEmail + " Selected Engineers");
}
searchFromArray(arr: Engineer[], input: string) {
let matches: Engineer[] = [], i: number;
for (i = 0; i < arr.length; i++) {
if (arr[i].user.email.match(input)) {
matches.push(arr[i]);
}
}
return matches;
};
submitHandle() {
if (this.ticket != null) {
console.log(this.complaintForm.get('ticket')!.get('engineer')!.get('id')!.value);
if (this.loggedInUser.role == 'manager') {
let engineer: Engineer = new Engineer();
engineer.id = this.complaintForm.get('ticket')!.get('engineer')!.get('id')!.value;
this.ticket.engineer = engineer;
this.updateTicket(this.ticket);
}
else if (this.loggedInUser.role == 'engineer') {
this.ticket.status = this.complaintForm.get('ticket')!.get('status')!.value;
this.updateTicket(this.ticket);
}
let comments: string = '';
comments = this.complaintForm.get('notes')!.get('comments')!.value;
console.log(comments + "====>Comments");
if ((comments.trim().length > 0)) {
let notes: Notes = new Notes();
notes.comments = comments;
notes.isCustomer = (this.loggedInUser.role == 'customer');
notes.updatedBy = this.loggedInUser.email;
notes.ticket = this.ticket;
this.ticketService.saveNotes(notes).subscribe({
next: response => {
alert(`Ticket# ${JSON.stringify(response)} updated successfully`)
},
error: err => {
alert(`There was an error: ${err.message}`);
console.log(err.message);
}
})
}
}
else {
let ticket: Ticket = this.complaintForm.controls['ticket'].value;
ticket.customer = this.loggedInCustomer;
ticket.engineer != null;
console.log(ticket);
this.ticketService.raiseNewTicket(ticket).subscribe({
next: response => {
alert(`Your ticket has been submitted! Ticket number: ${JSON.stringify(response)}`);
},
error: err => {
alert(`There was an error: ${err.message}`);
console.log(err.message);
}
});
}
}
updateTicket(ticket: Ticket) {
this.ticketService.raiseNewTicket(ticket).subscribe({
next: response => {
alert(`Ticket# ${JSON.stringify(response)} updated successfully`)
},
error: err => {
alert(`There was an error: ${err.message}`);
console.log(err.message);
}
})
}
}
<!-- <p>ticket-list works!</p> -->
<div class="container">
<h2 class="m-3">List of Tickets</h2>
<table class="table table-secondary table-bordered table-hover text-center table-sm">
<thead>
<tr>
<th>Customer Id</th>
<th>ticket Id</th>
<th>Customer Name</th>
<th>Service Type</th>
<th>Issue type</th>
<th>Address</th>
<th>Pincode</th>
<th>Status</th>
<th>Assigned to</th>
<th>Actions</th>
</tr>
</thead>
<tbody>
<tr *ngFor="let ticket of tickets">
<td>
{{ticket.customer.id}}
</td>
<td>
{{ticket.id}}
</td>
<td>
{{ticket.customer.user.fullName}}
</td>
<td>
{{ticket.serviceType}}
</td>
<td>
{{ticket.issueType}}
</td>
<td>
{{ticket.address}}
</td>
<td>
{{ticket.zipcode}}
</td>
<td>
{{ticket.status}}
</td>
<td *ngIf="ticket.engineer!=null">
{{ticket.engineer.user.fullName}}
</td>
<td class="text-center" *ngIf="ticket.engineer==null">
-
</td>
<td>
<a class="btn btn-sm btn-info"
[routerLink]="['/viewTicket',ticket.id]">View</a>&nbsp;
<a *ngIf="loggedInUser.role=='manager'" class="btn btn-sm btn-info"
[routerLink]="[ '/assign-engineer', routeParam ]">Assign</a>
<a
*ngIf="loggedInUser.role=='customer'&&(ticket.status=='RESOLVED')||(ticket.status=='ESCALATED')"
class="btn btn-sm btn-info" [routerLink]="[ '/feedback', ticket.id
]">Give Feedback</a>
</td>
</tr>
</tbody>
</table>
</div>
import { Component } from '@angular/core';
import { Ticket } from 'src/app/classes/ticket';
import { User } from 'src/app/classes/user';
import { LoginService } from 'src/app/services/login.service';
import { TicketService } from 'src/app/services/ticket.service';
@Component({
selector: 'app-ticket-list',
templateUrl: './ticket-list.component.html',
styleUrls: ['./ticket-list.component.css']
})
export class TicketListComponent {
tickets: Ticket[] = [];
selectedTicket!: Ticket;
loggedInUser!: User;
routeParam: any;
constructor(
private ticketService: TicketService,
private loginService: LoginService
) { }
ngOnInit(): void {
this.loginService.loggedInUser.subscribe(data => {
console.log("line 22");
this.loggedInUser != data;
if (data!.role == 'customer') {
console.log("line 25");
this.loginService.loggedInCustomer.subscribe(data2 =>
this.getTicketsByCustomerId(data2!.id));
console.log("line 27");
}
else {
this.getAllTickets();
console.log("line 31");
}
})
}
// End Of onInit
getTicketsByCustomerId(id: number) {
this.ticketService.getCustomerTickets(id).subscribe(data => {
this.tickets = data;
})
}
getAllTickets() {
this.ticketService.getAllTickets().subscribe(data => {
this.tickets = data;
});
}
}
<!-- <p>user-list works!</p> -->
<div class="container">
<!-- List of Customers -->
<!-- ****************** -->
<div *ngIf="role=='customer'">
<h2 class="text-center">List of Customers</h2>
<table class="table table-success table-striped">
<thead>
<tr>
<th>User Id</th>
<th>Customer Id</th>
<th>Full Name</th>
<th>Email</th>
<th>Role</th>
<th>Address</th>
<th>Services</th>
<th>Pincode</th>
<th>Actions</th>
</tr>
</thead>
<tbody>
<tr *ngFor="let customer of customers">
<td>
{{customer.user.id}}
</td>
<td>
{{customer.id}}
</td>
<td>
{{customer.user.fullName}}
</td>
<td>
{{customer.user.email}}
</td>
<td>
{{customer.user.role}}
</td>
<td>
{{customer.address}}
</td>
<td>
{{customer.serviceType}}
</td>
<td>
{{customer.zipcode}}
</td>
<td>
<a class="btn btn-sm btn-info"
[routerLink]="['/editUser',customer.id,customer.user.role]">Edit</a>&nbsp;
<button class="btn btn-sm btn-danger"
(click)="deleteUser(customer.id,customer.user.role)">Delete</button>
</td>
</tr>
</tbody>
</table>
</div>
<!-- List for Engineers -->
<!-- ****************** -->
<div *ngIf="role=='engineer'">
<h2 class="text-center">List of Engineers</h2>
<table class="table table-success table-striped">
<thead>
<tr>
<th>User Id</th>
<th>Engineer Id</th>
<th>Full Name</th>
<th>Email</th>
<th>Role</th>
<th>Pincode</th>
<th>Actions</th>
</tr>
</thead>
<tbody>
<tr *ngFor="let engineer of engineers">
<td>
{{engineer.user.id}}
</td>
<td>
{{engineer.id}}
</td>
<td>
{{engineer.user.fullName}}
</td>
<td>
{{engineer.user.email}}
</td>
<td>
{{engineer.user.role}}
</td>
<td>
{{engineer.zipcode}}
</td>
<td>
<a class="btn btn-sm btn-info"
[routerLink]="['/editUser',engineer.id,engineer.user.role]">Edit</a>&nbsp;
<button class="btn btn-sm btn-danger"
(click)="deleteUser(engineer.id,engineer.user.role)">Delete</button>
</td>
</tr>
</tbody>
</table>
</div>
<!-- List for Managers -->
<!-- ****************** -->
<div *ngIf="role=='manager'">
<h2 class="text-center">List of Managers</h2>
<table class="table table-success table-striped">
<thead>
<tr>
<th>User Id</th>
<th>Manager Id</th>
<th>Full Name</th>
<th>Email</th>
<th>Role</th>
<th>Pincodes</th>
<th>Actions</th>
</tr>
</thead>
<tbody>
<tr *ngFor="let manager of managers">
<td>
{{manager.user.id}}
</td>
<td>
{{manager.id}}
</td>
<td>
{{manager.user.fullName}}
</td>
<td>
{{manager.user.email}}
</td>
<td>
{{manager.user.role}}
</td>
<td>
{{manager.zipcode}}
</td>
<td>
<a class="btn btn-sm btn-info"
[routerLink]="['/editUser',manager.id,manager.user.role]">Edit</a>
&nbsp;
<!-- <button class="btn btn-sm btn-danger"
(click)="deleteUser(engineer.id,engineer.user.role)">Delete</button> -->
</td>
</tr>
</tbody>
</table>
</div>
</div>
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Customer } from 'src/app/classes/customer';
import { Engineer } from 'src/app/classes/engineer';
import { Manager } from 'src/app/classes/manager';
import { UserService } from 'src/app/services/user.service';
@Component({
selector: 'app-user-list',
templateUrl: './user-list.component.html',
styleUrls: ['./user-list.component.css']
})
export class UserListComponent {
customers: Customer[] = [];
engineers: Engineer[] = [];
managers: Manager[] = [];
role: string = '';
constructor(
private userService: UserService,
private route: ActivatedRoute
) { }
ngOnInit(): void {
this.route.paramMap.subscribe(() => {
this.loadusers();
})
}
loadusers() {
if (this.route.snapshot.paramMap.has('role')) {
this.role = this.route.snapshot.paramMap.get('role')!;
if (this.role == 'customer') {
this.userService.getCustomers().subscribe(data => [
this.customers = data
]);
}
if (this.role == 'engineer') {
this.userService.getEngineers().subscribe(data => {
this.engineers = data;
});
}
if (this.role == 'manager') {
this.userService.getManagers().subscribe(data => {
this.managers = data;
});
}
}
}
deleteUser(id: number, role: string) {
this.userService.deleteCustomer(id).subscribe(data => {
alert(`User ${data} has been deleted successfully`)
this.loadusers();
});
}
}
