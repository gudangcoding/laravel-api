## Dokumentasi
1. BUAT PROJECT VIA LARAGON<br>
2. Masuk ke root project kemudian buka via vscode lalu intall library swagger<br>
   <code>composer require "darkaonline/l5-swagger"</code><br>
   <code>php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"</code><br>
3. Buat Model artikel<br>
   	<code>php artisan make:model Article -m</code>
    Ubah File Model<br>
    <code>class Article extends Model
    {
        use HasFactory;
    
        protected $fillable = ['title', 'content', 'status'];
    }
    </code><br>
4. Ubah file migration artikel<br>
    <code>Schema::create('articles', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('content')->nullable();
            $table->enum('status', ['Published', 'Draft'])->default('Draft');
            $table->timestamps();
        });
   </code><br>
  lalu migrate <br>
  <code>php artisan migrate</code>
5. buat content dummy dengan factory<br>
   <code>php artisan make:factory Article</code><br>
   ubah file factory<br>
   <code>public function definition()
    {
        return [
            'title'   => $this->faker->realText($maxNbChars = 50, $indexSize = 2),
            'content' => $this->faker->paragraph(10, false),
            'status'  => $this->faker->randomElement(['Published', 'Draft'])
        ];
    }
    </code><br>
    Tambahkan ke seeder<br>
    <code>public function run()
    {
        //seed users table
        \App\Models\User::factory(2)->create();
        //seed articles table
        \App\Models\Article::factory(50)->create();
    }
    </code><br>
    generate datanya dengan seeder<br>
    <code>php artisan db:seed</code><br>

6. Ubah file Controller.php<br>
   <code><?php
    //app/Http/Controllers/Controller.php
    
    namespace App\Http\Controllers;
    
    use Illuminate\Foundation\Auth\Access\AuthorizesRequests;
    use Illuminate\Foundation\Bus\DispatchesJobs;
    use Illuminate\Foundation\Validation\ValidatesRequests;
    use Illuminate\Routing\Controller as BaseController;
    
    /**
    * @OA\Info(
    *     version="1.0.0",
    *     title="Kodementor Api Documentation",
    *     description="Kodementor Api Documentation",
    *     @OA\Contact(
    *         name="Vijay Rana",
    *         email="info@kodementor.com"
    *     ),
    *     @OA\License(
    *         name="Apache 2.0",
    *         url="http://www.apache.org/licenses/LICENSE-2.0.html"
    *     )
    * ),
    * @OA\Server(
    *     url="/api/v1",
    * ),
    */
    class Controller extends BaseController
    {
        use AuthorizesRequests, DispatchesJobs, ValidatesRequests;
    }
</code><br>
7. <code>generate swagger php artisan l5-swagger:generate</code><br>
akses dokumentasi<br> 
http://lara-api.app/api/documentation<br>
sumber : https://kodementor.com/laravel-api-documentation-with-swagger/
