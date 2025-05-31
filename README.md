# 👋 Hi there, I'm Alperen Ozdemir

<div align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&duration=3000&pause=1000&color=00D9FF&center=true&vCenter=true&width=600&lines=Full+Stack+Developer;Backend+Specialist;DevOps+Enthusiast;Building+Scalable+Solutions" alt="Typing SVG" />
</div>

<div align="center">
  
**Experienced software developer with 5+ years in modern web technologies and infrastructure solutions.**  
**Deep expertise in PHP ecosystem and containerization technologies.**

</div>

---

## 🚀 Tech Stack

<div align="center">

### **Backend Development**
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Symfony](https://img.shields.io/badge/Symfony-000000?style=for-the-badge&logo=symfony&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)

### **Frontend Development**
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)

### **Database Management**
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Microsoft SQL Server](https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white)

### **DevOps & Infrastructure**
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326ce5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![IIS](https://img.shields.io/badge/IIS-5C2D91?style=for-the-badge&logo=microsoft&logoColor=white)

### **Tools & Security**
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Composer](https://img.shields.io/badge/Composer-885630?style=for-the-badge&logo=composer&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)

</div>

---

---

# 🏗️ MVC Architecture Comparison

**Model-View-Controller pattern across different frameworks**

---

## 📋 Overview

MVC (Model-View-Controller) is a software architectural pattern that separates an application into three interconnected components. Here's how it's implemented across different frameworks:

---

## 🐘 Pure PHP MVC

### Project Structure
```
project/
├── app/
│   ├── Controllers/
│   │   ├── BaseController.php
│   │   ├── HomeController.php
│   │   └── UserController.php
│   ├── Models/
│   │   ├── BaseModel.php
│   │   └── User.php
│   └── Views/
│       ├── layouts/
│       │   └── main.php
│       ├── home/
│       │   └── index.php
│       └── users/
│           ├── index.php
│           └── create.php
├── config/
│   ├── database.php
│   └── routes.php
├── public/
│   ├── index.php
│   ├── css/
│   └── js/
└── vendor/
```

### Model Example
```php
<?php
// app/Models/User.php
class User extends BaseModel
{
    protected $table = 'users';
    protected $fillable = ['name', 'email', 'password'];

    public function __construct()
    {
        parent::__construct();
    }

    public function getAllUsers()
    {
        $sql = "SELECT * FROM {$this->table}";
        return $this->db->query($sql)->fetchAll(PDO::FETCH_ASSOC);
    }

    public function findById($id)
    {
        $sql = "SELECT * FROM {$this->table} WHERE id = :id";
        $stmt = $this->db->prepare($sql);
        $stmt->bindParam(':id', $id);
        $stmt->execute();
        return $stmt->fetch(PDO::FETCH_ASSOC);
    }

    public function create($data)
    {
        $columns = implode(',', array_keys($data));
        $placeholders = ':' . implode(', :', array_keys($data));
        
        $sql = "INSERT INTO {$this->table} ({$columns}) VALUES ({$placeholders})";
        $stmt = $this->db->prepare($sql);
        
        foreach ($data as $key => $value) {
            $stmt->bindValue(":{$key}", $value);
        }
        
        return $stmt->execute();
    }
}
```

### Controller Example
```php
<?php
// app/Controllers/UserController.php
class UserController extends BaseController
{
    private $userModel;

    public function __construct()
    {
        $this->userModel = new User();
    }

    public function index()
    {
        $users = $this->userModel->getAllUsers();
        $this->render('users/index', ['users' => $users]);
    }

    public function show($id)
    {
        $user = $this->userModel->findById($id);
        
        if (!$user) {
            header("HTTP/1.0 404 Not Found");
            $this->render('errors/404');
            return;
        }
        
        $this->render('users/show', ['user' => $user]);
    }

    public function create()
    {
        if ($_SERVER['REQUEST_METHOD'] === 'POST') {
            $data = [
                'name' => $_POST['name'] ?? '',
                'email' => $_POST['email'] ?? '',
                'password' => password_hash($_POST['password'] ?? '', PASSWORD_DEFAULT)
            ];
            
            if ($this->userModel->create($data)) {
                header('Location: /users');
                exit;
            }
        }
        
        $this->render('users/create');
    }
}
```

### View Example
```php
<!-- app/Views/users/index.php -->
<?php include_once '../app/Views/layouts/header.php'; ?>

<div class="container">
    <h1>Users List</h1>
    
    <a href="/users/create" class="btn btn-primary">Add New User</a>
    
    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <?php foreach ($users as $user): ?>
            <tr>
                <td><?= htmlspecialchars($user['id']) ?></td>
                <td><?= htmlspecialchars($user['name']) ?></td>
                <td><?= htmlspecialchars($user['email']) ?></td>
                <td>
                    <a href="/users/<?= $user['id'] ?>">View</a>
                    <a href="/users/<?= $user['id'] ?>/edit">Edit</a>
                </td>
            </tr>
            <?php endforeach; ?>
        </tbody>
    </table>
</div>

<?php include_once '../app/Views/layouts/footer.php'; ?>
```

---

## 🔴 Laravel MVC

### Project Structure
```
laravel-app/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Controller.php
│   │   │   └── UserController.php
│   │   ├── Middleware/
│   │   └── Requests/
│   ├── Models/
│   │   └── User.php
│   └── Providers/
├── resources/
│   └── views/
│       ├── layouts/
│       │   └── app.blade.php
│       └── users/
│           ├── index.blade.php
│           └── create.blade.php
├── routes/
│   ├── web.php
│   └── api.php
└── database/
    ├── migrations/
    └── seeders/
```

### Model Example (Eloquent ORM)
```php
<?php
// app/Models/User.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    protected $hidden = [
        'password',
        'remember_token',
    ];

    protected $casts = [
        'email_verified_at' => 'datetime',
        'password' => 'hashed',
    ];

    // Relationships
    public function posts()
    {
        return $this->hasMany(Post::class);
    }

    public function profile()
    {
        return $this->hasOne(Profile::class);
    }

    // Scopes
    public function scopeActive($query)
    {
        return $query->where('status', 'active');
    }

    // Accessors & Mutators
    public function getFullNameAttribute()
    {
        return $this->first_name . ' ' . $this->last_name;
    }
}
```

### Controller Example
```php
<?php
// app/Http/Controllers/UserController.php
namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Http\RedirectResponse;
use Illuminate\View\View;

class UserController extends Controller
{
    public function index(): View
    {
        $users = User::with('profile')
                    ->active()
                    ->paginate(15);

        return view('users.index', compact('users'));
    }

    public function show(User $user): View
    {
        $user->load(['posts', 'profile']);
        
        return view('users.show', compact('user'));
    }

    public function create(): View
    {
        return view('users.create');
    }

    public function store(Request $request): RedirectResponse
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:users',
            'password' => 'required|string|min:8|confirmed',
        ]);

        $user = User::create($validated);

        return redirect()
            ->route('users.show', $user)
            ->with('success', 'User created successfully!');
    }

    public function edit(User $user): View
    {
        return view('users.edit', compact('user'));
    }

    public function update(Request $request, User $user): RedirectResponse
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:users,email,' . $user->id,
        ]);

        $user->update($validated);

        return redirect()
            ->route('users.show', $user)
            ->with('success', 'User updated successfully!');
    }

    public function destroy(User $user): RedirectResponse
    {
        $user->delete();

        return redirect()
            ->route('users.index')
            ->with('success', 'User deleted successfully!');
    }
}
```

### View Example (Blade Template)
```blade
{{-- resources/views/users/index.blade.php --}}
@extends('layouts.app')

@section('title', 'Users List')

@section('content')
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1>Users</h1>
                <a href="{{ route('users.create') }}" class="btn btn-primary">
                    Add New User
                </a>
            </div>

            @if(session('success'))
                <div class="alert alert-success">
                    {{ session('success') }}
                </div>
            @endif

            <div class="card">
                <div class="card-body">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Created At</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            @forelse($users as $user)
                            <tr>
                                <td>{{ $user->id }}</td>
                                <td>{{ $user->name }}</td>
                                <td>{{ $user->email }}</td>
                                <td>{{ $user->created_at->format('M d, Y') }}</td>
                                <td>
                                    <a href="{{ route('users.show', $user) }}" class="btn btn-sm btn-info">View</a>
                                    <a href="{{ route('users.edit', $user) }}" class="btn btn-sm btn-warning">Edit</a>
                                    
                                    <form action="{{ route('users.destroy', $user) }}" method="POST" class="d-inline">
                                        @csrf
                                        @method('DELETE')
                                        <button type="submit" class="btn btn-sm btn-danger" 
                                                onclick="return confirm('Are you sure?')">Delete</button>
                                    </form>
                                </td>
                            </tr>
                            @empty
                            <tr>
                                <td colspan="5" class="text-center">No users found.</td>
                            </tr>
                            @endforelse
                        </tbody>
                    </table>

                    {{ $users->links() }}
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
```

### Routes Example
```php
<?php
// routes/web.php
use App\Http\Controllers\UserController;

Route::get('/', function () {
    return view('welcome');
});

Route::resource('users', UserController::class);

// Custom routes
Route::get('/users/{user}/posts', [UserController::class, 'posts'])->name('users.posts');
Route::post('/users/{user}/activate', [UserController::class, 'activate'])->name('users.activate');
```

---

## 🎼 Symfony MVC

### Project Structure
```
symfony-app/
├── src/
│   ├── Controller/
│   │   └── UserController.php
│   ├── Entity/
│   │   └── User.php
│   ├── Repository/
│   │   └── UserRepository.php
│   └── Form/
│       └── UserType.php
├── templates/
│   ├── base.html.twig
│   └── user/
│       ├── index.html.twig
│       └── show.html.twig
├── config/
│   ├── routes.yaml
│   └── packages/
└── migrations/
```

### Entity (Model) Example
```php
<?php
// src/Entity/User.php
namespace App\Entity;

use App\Repository\UserRepository;
use Doctrine\Common\Collections\ArrayCollection;
use Doctrine\Common\Collections\Collection;
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Security\Core\User\UserInterface;
use Symfony\Component\Validator\Constraints as Assert;

#[ORM\Entity(repositoryClass: UserRepository::class)]
#[ORM\Table(name: 'users')]
class User implements UserInterface
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private ?int $id = null;

    #[ORM\Column(type: 'string', length: 180, unique: true)]
    #[Assert\NotBlank]
    #[Assert\Email]
    private ?string $email = null;

    #[ORM\Column(type: 'json')]
    private array $roles = [];

    #[ORM\Column(type: 'string')]
    #[Assert\NotBlank]
    #[Assert\Length(min: 2, max: 255)]
    private ?string $name = null;

    #[ORM\Column(type: 'string')]
    private ?string $password = null;

    #[ORM\OneToMany(mappedBy: 'author', targetEntity: Post::class)]
    private Collection $posts;

    #[ORM\Column(type: 'datetime_immutable')]
    private ?\DateTimeImmutable $createdAt = null;

    public function __construct()
    {
        $this->posts = new ArrayCollection();
        $this->createdAt = new \DateTimeImmutable();
    }

    // Getters and Setters
    public function getId(): ?int
    {
        return $this->id;
    }

    public function getEmail(): ?string
    {
        return $this->email;
    }

    public function setEmail(string $email): self
    {
        $this->email = $email;
        return $this;
    }

    public function getUserIdentifier(): string
    {
        return (string) $this->email;
    }

    public function getRoles(): array
    {
        $roles = $this->roles;
        $roles[] = 'ROLE_USER';

        return array_unique($roles);
    }

    public function setRoles(array $roles): self
    {
        $this->roles = $roles;
        return $this;
    }

    public function getName(): ?string
    {
        return $this->name;
    }

    public function setName(string $name): self
    {
        $this->name = $name;
        return $this;
    }

    public function getPassword(): string
    {
        return $this->password;
    }

    public function setPassword(string $password): self
    {
        $this->password = $password;
        return $this;
    }

    public function getPosts(): Collection
    {
        return $this->posts;
    }

    public function addPost(Post $post): self
    {
        if (!$this->posts->contains($post)) {
            $this->posts[] = $post;
            $post->setAuthor($this);
        }

        return $this;
    }

    public function getCreatedAt(): ?\DateTimeImmutable
    {
        return $this->createdAt;
    }

    public function eraseCredentials(): void
    {
        // Clear temporary sensitive data
    }
}
```

### Repository Example
```php
<?php
// src/Repository/UserRepository.php
namespace App\Repository;

use App\Entity\User;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;
use Doctrine\ORM\Tools\Pagination\Paginator;

class UserRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, User::class);
    }

    public function findAllPaginated(int $page = 1, int $limit = 10): Paginator
    {
        $query = $this->createQueryBuilder('u')
            ->orderBy('u.createdAt', 'DESC')
            ->getQuery();

        $paginator = new Paginator($query);
        $paginator->getQuery()
            ->setFirstResult(($page - 1) * $limit)
            ->setMaxResults($limit);

        return $paginator;
    }

    public function findByEmail(string $email): ?User
    {
        return $this->createQueryBuilder('u')
            ->andWhere('u.email = :email')
            ->setParameter('email', $email)
            ->getQuery()
            ->getOneOrNullResult();
    }

    public function findActiveUsers(): array
    {
        return $this->createQueryBuilder('u')
            ->andWhere('u.roles LIKE :role')
            ->setParameter('role', '%ROLE_USER%')
            ->orderBy('u.name', 'ASC')
            ->getQuery()
            ->getResult();
    }

    public function save(User $entity, bool $flush = false): void
    {
        $this->getEntityManager()->persist($entity);

        if ($flush) {
            $this->getEntityManager()->flush();
        }
    }

    public function remove(User $entity, bool $flush = false): void
    {
        $this->getEntityManager()->remove($entity);

        if ($flush) {
            $this->getEntityManager()->flush();
        }
    }
}
```

### Controller Example
```php
<?php
// src/Controller/UserController.php
namespace App\Controller;

use App\Entity\User;
use App\Form\UserType;
use App\Repository\UserRepository;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\PasswordHasher\Hasher\UserPasswordHasherInterface;

#[Route('/users')]
class UserController extends AbstractController
{
    public function __construct(
        private UserRepository $userRepository,
        private EntityManagerInterface $entityManager
    ) {}

    #[Route('/', name: 'user_index', methods: ['GET'])]
    public function index(Request $request): Response
    {
        $page = $request->query->getInt('page', 1);
        $users = $this->userRepository->findAllPaginated($page);

        return $this->render('user/index.html.twig', [
            'users' => $users,
            'current_page' => $page,
        ]);
    }

    #[Route('/new', name: 'user_new', methods: ['GET', 'POST'])]
    public function new(Request $request, UserPasswordHasherInterface $passwordHasher): Response
    {
        $user = new User();
        $form = $this->createForm(UserType::class, $user);
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            // Hash the password
            $hashedPassword = $passwordHasher->hashPassword(
                $user,
                $form->get('plainPassword')->getData()
            );
            $user->setPassword($hashedPassword);

            $this->userRepository->save($user, true);

            $this->addFlash('success', 'User created successfully!');

            return $this->redirectToRoute('user_index');
        }

        return $this->render('user/new.html.twig', [
            'user' => $user,
            'form' => $form,
        ]);
    }

    #[Route('/{id}', name: 'user_show', methods: ['GET'])]
    public function show(User $user): Response
    {
        return $this->render('user/show.html.twig', [
            'user' => $user,
        ]);
    }

    #[Route('/{id}/edit', name: 'user_edit', methods: ['GET', 'POST'])]
    public function edit(Request $request, User $user): Response
    {
        $form = $this->createForm(UserType::class, $user);
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            $this->userRepository->save($user, true);

            $this->addFlash('success', 'User updated successfully!');

            return $this->redirectToRoute('user_index');
        }

        return $this->render('user/edit.html.twig', [
            'user' => $user,
            'form' => $form,
        ]);
    }

    #[Route('/{id}', name: 'user_delete', methods: ['POST'])]
    public function delete(Request $request, User $user): Response
    {
        if ($this->isCsrfTokenValid('delete'.$user->getId(), $request->request->get('_token'))) {
            $this->userRepository->remove($user, true);
            $this->addFlash('success', 'User deleted successfully!');
        }

        return $this->redirectToRoute('user_index');
    }
}
```

### View Example (Twig Template)
```twig
{# templates/user/index.html.twig #}
{% extends 'base.html.twig' %}

{% block title %}Users List{% endblock %}

{% block body %}
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1>Users</h1>
                <a href="{{ path('user_new') }}" class="btn btn-primary">
                    Add New User
                </a>
            </div>

            {% for message in app.flashes('success') %}
                <div class="alert alert-success">
                    {{ message }}
                </div>
            {% endfor %}

            <div class="card">
                <div class="card-body">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Created At</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            {% for user in users %}
                            <tr>
                                <td>{{ user.id }}</td>
                                <td>{{ user.name }}</td>
                                <td>{{ user.email }}</td>
                                <td>{{ user.createdAt|date('M d, Y') }}</td>
                                <td>
                                    <a href="{{ path('user_show', {id: user.id}) }}" class="btn btn-sm btn-info">View</a>
                                    <a href="{{ path('user_edit', {id: user.id}) }}" class="btn btn-sm btn-warning">Edit</a>
                                    
                                    <form method="post" action="{{ path('user_delete', {id: user.id}) }}" style="display:inline;">
                                        <input type="hidden" name="_token" value="{{ csrf_token('delete' ~ user.id) }}">
                                        <button class="btn btn-sm btn-danger" onclick="return confirm('Are you sure?')">Delete</button>
                                    </form>
                                </td>
                            </tr>
                            {% else %}
                            <tr>
                                <td colspan="5" class="text-center">No users found.</td>
                            </tr>
                            {% endfor %}
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

---

## 🐍 Django MVT (Model-View-Template)

### Project Structure
```
django-app/
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── users/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── forms.py
│   ├── migrations/
│   └── templates/
│       └── users/
│           ├── index.html
│           └── detail.html
└── manage.py
```

### Model Example
```python
# users/models.py
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.urls import reverse
from django.utils import timezone

class User(AbstractUser):
    email = models.EmailField(unique=True)
    name = models.CharField(max_length=255)
    bio = models.TextField(blank=True, null=True)
    avatar = models.ImageField(upload_to='avatars/', blank=True, null=True)
    is_verified = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['username', 'name']

    class Meta:
        db_table = 'users'
        ordering = ['-created_at']
        verbose_name = 'User'
        verbose_name_plural = 'Users'

    def __str__(self):
        return self.name or self.username

    def get_absolute_url(self):
        return reverse('users:detail', kwargs={'pk': self.pk})

    def get_full_name(self):
        return self.name or f"{self.first_name} {self.last_name}".strip()

    def get_posts_count(self):
        return self.posts.count()

    @property
    def is_active_user(self):
        return self.is_active and self.is_verified

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE, related_name='posts')
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    is_published = models.BooleanField(default=False)

    class Meta:
        ordering = ['-created_at']

    def __str__(self):
        return self.title

class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='profile')
    website = models.URLField(blank=True, null=True)
    location = models.CharField(max_length=100, blank=True, null=True)
    birth_date = models.DateField(blank=True, null=True)

    def __str__(self):
        return f"{self.user.name}'s Profile"
```

### Views Example
```python
# users/views.py
from django.shortcuts import render, get_object_or_404, redirect
from django.contrib import messages
from django.contrib.auth.decorators import login_required
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from django.core.paginator import Paginator
from django.db.models import Q
from .models import User, Post
from .forms import UserForm, UserUpdateForm

# Function-based views
def user_list(request):
    search_query = request.GET.get('search', '')
    users = User.objects.filter(is_active=True)
    
    if search_query:
        users = users.filter(
            Q(name__icontains=search_query) | 
            Q(email__icontains=search_query)
        )
    
    paginator = Paginator(users, 10)
    page_number = request.GET.get('page')
    page_obj = paginator.get_page(page_number)
    
    context = {
        'page_obj': page_obj,
        'search_query': search_query,
    }
    return render(request, 'users/index.html', context)

def user_detail(request, pk):
    user = get_object_or_404(User, pk=pk)
    posts = user.posts.filter(is_published=True)[:5]
    
    context = {
        'user': user,
        'posts': posts,
    }
    return render(request, 'users/detail.html', context)

@login_required
def user_create(request):
    if request.method == 'POST':
        form = UserForm(request.POST, request.FILES)
        if form.is_valid():
            user = form.save()
            messages.success(request, 'User created successfully!')
            return redirect('users:detail', pk=user.pk)
    else:
        form = UserForm()
    
    return render(request, 'users/create.html', {'form': form})

@login_required
def user_update(request, pk):
    user = get_object_or_404(User, pk=pk)
    
    if request.method == 'POST':
        form = UserUpdateForm(request.POST, request.FILES, instance=user)
        if form.is_valid():
            form.save()
            messages.success(request, 'User updated successfully!')
            return redirect('users:detail', pk=user.pk)
    else:
        form = UserUpdateForm(instance=user)
    
    return render(request, 'users/update.html', {'form': form, 'user': user})

@login_required
def user_delete(request, pk):
    user = get_object_or_404(User, pk=pk)
    
    if request.method == 'POST':
        user.delete()
        messages.success(request, 'User deleted successfully!')
        return redirect('users:index')
    
    return render(request, 'users/delete.html', {'user': user})

# Class-based views
class UserListView(ListView):
    model = User
    template_name = 'users/index.html
---

## 💼 Core Expertise

<div align="center">

| Area | Technologies | Experience |
|------|-------------|------------|
| 🛡️ **Web Security** | WAF Configuration, Security Audits | Advanced |
| 🚀 **API Development** | RESTful APIs, GraphQL | Expert |
| 📊 **Database Design** | Query Optimization, Schema Design | Advanced |
| 🐳 **Containerization** | Docker, Kubernetes, Microservices | Expert |
| ⚙️ **Server Management** | Linux/Windows, Nginx, IIS | Advanced |
| 💻 **Frontend** | React, Modern JavaScript | Intermediate |

</div>

---

## 📊 GitHub Analytics

<div align="center">
  <img width="49%" src="https://github-readme-stats.vercel.app/api?username=fetchAlp&show_icons=true&theme=tokyonight&hide_border=true&bg_color=0D1117&title_color=00D9FF&icon_color=00D9FF&text_color=FFFFFF" alt="GitHub Stats" />
  <img width="49%" src="https://github-readme-stats.vercel.app/api/top-langs/?username=fetchAlp&layout=compact&theme=tokyonight&hide_border=true&bg_color=0D1117&title_color=00D9FF&text_color=FFFFFF" alt="Top Languages" />
</div>

<div align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=fetchAlp&theme=tokyonight&hide_border=true&background=0D1117&stroke=00D9FF&ring=00D9FF&fire=FF6B6B&currStreakLabel=FFFFFF" alt="GitHub Streak" />
</div>

---

## 🎯 Current Projects

<div align="center">

| Project | Stack | Status | Description |
|---------|-------|--------|-------------|
| 🛒 **Enterprise E-Commerce** | Laravel + React + Docker | 🚧 In Progress | Full-stack e-commerce platform with microservices |
| ⚡ **API Gateway** | Kubernetes + Nginx | 🔧 Architecture | High-performance gateway for distributed systems |
| 📱 **Headless CMS** | Symfony 6 + PostgreSQL | 🎯 Planning | Content management system with API-first approach |
| 🛡️ **Security Scanner** | PHP + Docker | ✅ Complete | Automated vulnerability assessment tool |

</div>

---

## 🏆 Key Achievements

<div align="center">

| Achievement | Impact | Technology |
|-------------|--------|------------|
| ⚡ **Performance Optimization** | 60% faster response times | Database tuning, Caching |
| 🛡️ **Security Implementation** | 50+ applications protected | WAF, Security audits |
| 🐳 **Container Migration** | Legacy system modernization | Docker, Kubernetes |
| 🚀 **API Architecture** | 10M+ daily requests handled | RESTful design, Load balancing |

</div>

---

## 🌐 Let's Connect

<div align="center">

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:aozdemir@yazilimciniz.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=About.me&logoColor=white)](https://yazilimciniz.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/alperen-ozdemir)

</div>

---

## 📈 Development Stats

<div align="center">

| Metric | Count | 
|--------|-------|
| 🗂️ **Total Repositories** | 50+ |
| 🔥 **Commits (2024)** | 1,200+ |
| 🐳 **Docker Images** | 15+ |
| ⚡ **API Endpoints** | 200+ |
| ☕ **Coffee Consumed** | ∞ |

</div>

---

## 💻 API Collaboration Request

<div align="center">

### 🔗 Want to work together? Here's my "API":

| Endpoint | Method | Response |
|----------|--------|----------|
| `/api/developer/availability` | GET | `{ "status": "available", "response_time": "< 24h" }` |
| `/api/developer/skills` | GET | `{ "php": "expert", "laravel": "expert", "docker": "advanced" }` |
| `/api/developer/hire` | POST | `{ "status": "interested", "coffee_required": true }` ☕ |
| `/api/developer/collaborate` | POST | `{ "status": "ready", "enthusiasm": "high" }` 🚀 |

**Ready to make something awesome together?**

</div>

---

<div align="center">

*"Clean code is not written by following a set of rules. Clean code is written by a programmer who cares."*

<img src="https://komarev.com/ghpvc/?username=fetchAlp&style=flat-square&color=blue" alt="Profile Views"/>

**🚀 Always ready for new challenges and exciting projects!**

</div>
