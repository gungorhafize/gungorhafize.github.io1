Android Threading: All You Need to Know

Android, iş parçacığı yani thread oluşturma ve yönetmenin birçok yolunu sunar ve bunu daha da kolaylaştırmak için üçüncü parti kütüphaneler de mevcuttur. 
Ancak bu kadar çok seçenek varken doğru yaklaşımı seçmek oldukça kafa karıştırıcı olabilir.
Her Android geliştiricisi, bir noktada, uygulamalarında threadlerle uğraşmak zorundadır.
Uygulamanızın yanıt vermeye devam etmesi için, bloke yada crash edilmesine neden olabilecek herhangi bir işlemi gerçekleştirmek için ana iş parçacığını -main thread'i- kullanmaktan kaçınmak önemlidir.
Ağ işlemleri ve veritabanı çağrılarının yanı sıra belirli bileşenlerin yüklenmesi, main thread'de kaçınılması gereken yaygın işlem örnekleridir.
Ana iş parçacığında çağrıldıklarında eşzamanlı(senkron) olarak çağrılırlar; bu da işlem tamamlanana kadar UI'nin tamamen yanıtsız kalacağı anlamına gelir.
Bu nedenle, genellikle ayrı threadlerde gerçekleştirilirler, böylelikle gerçekleştirilirken UI'nin engellenmesini önler (yani, UI'den eşzamansız (asenkron) olarak gerçekleştirilirler).

Android geliştirmede iş parçacığı -thread- oluşturmanın zorunlu hale geldiği bazı yaygın senaryoları ve bu senaryolara uygulanabilecek bazı basit çözümleri görelim.

## Android'de Threading

Android'de, tüm thread bileşenlerini iki temel kategoriye ayırabilirsiniz:
1)Bir activity/fragmente bağlı olan threadler: Bu iş parçacıkları, aktivitenin/fragmentin yaşam döngüsüne bağlıdır ve aktivite/fragment yok edilir edilmez sonlandırılır.
2)Herhangi bir activity/fragmente bağlı olmayan diziler: Bu diziler, oluşturulduklarıactivity/fragmente (varsa) ömrü boyunca çalışmaya devam edebilir.

## Bir Aktiviteye/Fragment Eklenen Threading Bileşenleri:

*ASYNCTASK

AsyncTask, thread için en temel Android bileşenidir. Kullanımı basittir ve temel senaryolar için iyi olabilir.

public class MyActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        new MyTask().execute(url);
    }

    private class MyTask extends AsyncTask<String, Void, String> {

        @Override
        protected String doInBackground(String... params) {
            String url = params[0];
            return doSomeWork(url);
        }

        @Override
        protected void onPostExecute(String result) {
            super.onPostExecute(result);
            // do something with result 
        }
    }
}

Ancak, activity/fragment ömrünün ötesine geçmeye ihtiyacınız varsa, AsyncTask yetersiz kalır. Ekran döndürme kadar basit bir hareketin bile aktivitenin yok olmasına neden olabileceğini belirtmekte fayda var.

*LOADERS*

Yükleyiciler(Loaders) yukarıda bahsedilen problemin çözümüdür. Loaders etkinlik yok edildiğinde otomatik olarak durabilir ve etkinlik yeniden oluşturulduktan sonra kendilerini yeniden başlatabilir.
Genel olarak iki tür loader vardır: AsyncTaskLoader ve CursorLoader. Bu yazının ilerleyen bölümlerinde CursorLoader hakkında daha fazla bilgi edineceksiniz.

AsyncTaskLoader, AsyncTask'a benzer, ancak biraz daha karmaşıktır.

Örnek kullanım:

public class MyActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        getLoaderManager().initLoader(1, null, new MyLoaderCallbacks());
    }
    
    private class MyLoaderCallbacks implements LoaderManager.LoaderCallbacks {

        @Override
        public Loader onCreateLoader(int id, Bundle args) {
            return new MyLoader(ExampleActivity.this);
        }

        @Override
        public void onLoadFinished(Loader loader, Object data) {

        }

        @Override
        public void onLoaderReset(Loader loader) {

        }
    }

    private class MyLoader extends AsyncTaskLoader {

        public MyLoader(Context context) {
            super(context);
        }

        @Override
        public Object loadInBackground() {
            return doSomething();
        }
        
    }
}

## Bir Aktiviteye/Fragmente Eklenmeyen Thread Bileşenleri

*SERVICE*

Service, herhangi bir kullanıcı arabirimi olmadan uzun (veya potansiyel olarak uzun) işlemleri gerçekleştirmek için yararlı olan androidin temel 4 bileşeninden bir tanesidir.

public class MyService extends Service {

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        doLongProcessWork();
        stopSelf();

        return START_NOT_STICKY;
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
}

Service ile, çalışması tamamlandığında stopSelf() veya stopService() yöntemini çağırarak onu durdurmak sizin sorumluluğunuzdadır.

*INTENTSERVICE*

Service gibi IntentService de ayrı bir thread üzerinde çalışır ve işini tamamladıktan sonra kendini otomatik olarak durdurur.

 
IntentService genellikle herhangi bir kullanıcı arayüzüne eklenmesi gerekmeyen kısa görevler için kullanılır.


public class MyInternService extends IntentService {
    
    public MyInternService () {
        superMyInternService 
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        doShortWork();
    }
}

### Android'de 7 Threading Modeli

Kullanım Durumu No. 1: Sunucudan yanıt gerektirmeden ağ üzerinden istekte bulunma
Bazen, yanıtı hakkında endişelenmenize gerek kalmadan bir sunucuya bir API isteği göndermek isteyebilirsiniz.
 Örneğin, uygulamanızın back-endine push registration token gönderiyor olabilirsiniz.
Bu, ağ üzerinden bir istek yapmayı içerdiğinden, bunu main thread dışındaki bir thread ile yapmalısınız.

SEÇENEK 1: ASYNCTASK VEYA LOADER

Arama yapmak için AsyncTask veya LOADERS kullanabilirsiniz ve işe yarayacaktır. 

Ancak, AsyncTask ve LOADERS her ikisi de etkinliğin yaşam döngüsüne bağlıdır. 
Bu, ya İŞLEMİN yürütülmesini beklemeniz ve kullanıcının activityden ayrılmasını engellemeye çalışmanız ya da activity yok edilmeden önce yürütüleceğini ummanız gerektiği anlamına gelir.

SEÇENEK 2: SERVICE 

Service, herhangi bir activitye bağlı olmadığı için bu kullanım durumu için daha uygun olabilir. Bu nedenle, aktivite yok edildikten sonra bile ağ aramasına devam edebilecektir. Ayrıca, sunucudan yanıt alınması gerekmediğinden burada da bir service kısıtlanmış olmaz.


Ancak, UI iş parçacığında bir service çalışmaya başlayacağından, thread oluşturmayı yine de kendiniz yönetmeniz gerekir. Ağ araması tamamlandığında service'in durdurulduğundan da emin olmanız gerekir.

Bu, böyle basit bir eylem için gerekli olandan daha fazla çaba gerektirecektir.

SEÇENEK 3: INTENTSERVICE

Bu, bence, en iyi seçenek olacaktır.

IntentService herhangi bir activity bağlanmadığından ve UI olmayan bir iş parçacığında çalıştığından, buradaki ihtiyaçlarımıza mükemmel bir şekilde hizmet ediyor. 
Ayrıca, IntentService kendini otomatik olarak durdurur, bu nedenle manuel olarak yönetmeye de gerek yoktur.

Kullanım Durumu No. 2: Bir ağ araması yapmak ve sunucudan yanıt almak

Bu kullanım durumu muhtemelen biraz daha yaygındır. Örneğin, arka planda bir API'yi çağırmak ve ekrandaki alanları doldurmak için yanıtını kullanmak isteyebilirsiniz.

SEÇENEK 1: SERVICE YADA INTENTSERVICE
Bir Service veya bir IntentService, önceki kullanım durumu için iyi sonuç vermiş olsa da, bunları burada kullanmak iyi bir fikir olmaz. Bir Servisten veya bir IntentService'ten ana UI iş parçacığına veri almaya çalışmak, işleri çok karmaşık hale getirir.

SEÇENEK 2: ASYNCTASK VEYA LOADERS

İlk bakışta, AsyncTask veya loaders burada bariz çözüm gibi görünebilir. Kullanımı kolaydır - basit ve anlaşılır.
Ancak, AsyncTask veya yükleyicileri kullanırken, bazı ortak kodlar yazmanız gerektiğini fark edeceksiniz. 
Ayrıca, hata ayıklama, bu bileşenlerle büyük bir angarya haline gelir. Basit bir ağ aramasında bile olası istisnaların farkında olmanız, onları yakalamanız ve buna göre hareket etmeniz gerekir.
 Bu, yanıtımızı, olası hata bilgileriyle birlikte verileri içeren özel bir sınıf oluşturmaya zorlar, işlemin başarılı olup olmadığını gösterir. Bu da, her bir arama için yapılacak oldukça fazla iş demektir. Neyse ki, artık çok daha iyi ve daha basit bir çözüm mevcut: RxJava.

SEÇENEK 3: RXJAVA
Netflix tarafından geliştirilen kütüphane RxJava'yı duymuşsunuzdur. 
RxAndroid, Android'de RxJava'yı kullanmanıza izin verir ve asenkron tasklarla uğraşmayı kolaylaştırır. Android'de RxJava hakkında daha fazla bilgiyi buradan edinebilirsiniz.
RxJava provides two components: Observer and Subscriber.
Bir observer bazı actionları içeren bir bileşendir. Bu actionı gerçekleştirir ve başarılı olursa sonucu, başarısız olursa bir hata döndürür.
Subscriber ise gözlemlenebilir bir nesneye subscribe olarak sonucu (veya hatayı) alabilen bir bileşendir.
RxJava ile önce bir observable yaratırsınız:
Observable.create((ObservableOnSubscribe<Data>) e -> {
    Data data = mRestApi.getData();
    e.onNext(data);
})

Observable oluşturulduktan sonra, ona subscribe olabilirsiniz.

RxAndroid kütüphanesi ile, observable'daki eylemi gerçekleştirmek istediğiniz thread ve yanıt almak istediğiniz threadı (yani sonuç veya hata) kontrol edebilirsiniz.



Bu iki işlevle observable'ı zincirlersiniz:

.subscribeOn(Schedulers.io())
.observeOn(AndroidSchedulers.mainThread()

Schedulers eylemi belirli bir threadde yürüten bileşenlerdir.
AndroidSchedulers.mainThread(), main threadle ilişkili zamanlayıcıdır.

API çağrımızın mRestApi.getData() olduğu ve bir Data nesnesi döndürdüğü göz önüne alındığında, temel çağrı şöyle görünebilir:

Observable.create((ObservableOnSubscribe<Data>) e -> {
            try { 
                        Data data = mRestApi.getData();
                        e.onNext(data);
            } catch (Exception ex) {
                        e.onError(ex);
            }
})
.subscribeOn(Schedulers.io())
.observeOn(AndroidSchedulers.mainThread())
.subscribe(match -> Log.i(“rest api, "success"),
            throwable -> Log.e(“rest api, "error: %s" + throwable.getMessage()));

RxJava kullanmanın diğer faydalarına bile girmeden, RxJava'nın thread oluşturmanın karmaşıklığını ortadan kaldırarak daha olgun kodlar yazmamıza nasıl izin verdiğini zaten görebilirsiniz.

Kullanım Durumu No. 3: Ağ aramalarını zincirleme:

Sırayla gerçekleştirilmesi gereken ağ aramaları için (yani, her işlemin bir önceki işlemin yanıtına/sonucuna bağlı olduğu durumlarda), spagetti kod oluşturmamaya özellikle dikkat etmek gerekir.
Örneğin, önce başka bir API çağrısı yoluyla getirmeniz gereken bir belirteçle bir API çağrısı yapmanız gerekebilir.

OPTION 1: ASYNCTASK OR LOADERS

AsyncTask veya loader kullanmak kesinlikle spagetti koda yol açacaktır. Genel işlevselliği doğru bir şekilde elde etmek zor olacak ve projeniz boyunca çok büyük miktarda yedek ortak kod gerektirecektir.

OPTION 2: RXJAVA USING FLATMAP

RxJava'da flatMap operatörü, observable kaynaktan yayılan bir değer alır ve başka bir observable değeri döndürür. Bir observable oluşturabilir ve ardından ilkinden yayılan değeri kullanarak temelde onları zincirleyecek olan başka bir obsevable oluşturabilirsiniz.

Adım 1. Token'ı getiren observable oluşturun:

public Observable<String> getTokenObservable() {
    return Observable.create(subscriber -> {
        try {
            String token = mRestApi.getToken();
            subscriber.onNext(token);

        } catch (IOException e) {
            subscriber.onError(e);
        }
    });
}

Adım 2. Token kullanarak verileri alan observable oluşturun:

public Observable<String> getDataObservable(String token) {
    return Observable.create(subscriber -> {
        try {
            Data data = mRestApi.getData(token);
            subscriber.onNext(data);

        } catch (IOException e) {
            subscriber.onError(e);
        }
    });
}

Adım 3. İki observable'ı birbirine zincirleyin ve subscribe olun:

getTokenObservable()
.flatMap(new Function<String, Observable<Data>>() {
    @Override
    public Observable<Data> apply(String token) throws Exception {
        return getDataObservable(token);
    }
})
.subscribe(data -> {
    doSomethingWithData(data)
}, error -> handleError(e));

Bu yaklaşımın kullanımının ağ aramalarıyla sınırlı olmadığını unutmayın; bir sırayla, ancak ayrı threadler üzerinde çalıştırılması gereken herhangi bir eylem kümesiyle çalışabilir.

Yukarıdaki tüm kullanım durumları oldukça basittir. Threadler arasında geçiş, yalnızca her biri görevini bitirdikten sonra oldu. 
Daha gelişmiş senaryolar (örneğin, iki veya daha fazla iş parçacığının birbiriyle etkin bir şekilde iletişim kurması gerektiği) bu yaklaşımla da desteklenebilir.

Kullanım Durum No. 4: Başka birthreadden UI thread ile iletişim kurun

Bir dosya yüklemek ve tamamlandıktan sonra kullanıcı arayüzünü güncellemek istediğiniz bir senaryo düşünün.
Bir dosyanın yüklenmesi uzun sürebileceğinden kullanıcıyı bekletmeye gerek yoktur. Buradaki işlevi uygulamak için bir service ve muhtemelen IntentService kullanabilirsiniz.
Ancak bu durumda, daha büyük zorluk, dosya yüklemesi (ayrı bir threadde gerçekleştirilen) tamamlandıktan sonra UI threadde  bir yöntemi çağırabilmektir.
Öte yandan, bir Servisle daha fazla çalışma gerektiren servisi manuel olarak durdurmanız gerekecektir.
