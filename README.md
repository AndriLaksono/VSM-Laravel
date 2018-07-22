# VSM Laravel (Cosine Measurement)
Module VSM untuk membantu pencarian pada laravel menggunakan Information retrieval berbahasa indonesia.

## Getting Started
Untuk menggunakan model ini anda harus melakukan beberapa langkah setup.
* Import table [tb_katadasar.sql](http://www.google.com) ke database anda.
* Buat folder **Helper** dibawah folder *app/*
* Letakan **VSMProvider.php** dan **PreprocessingProvider.php** divawah folder *app/Provider*

### Prerequisites
Setelah melakukan setup anda dapat menggunakan pada controller
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\DB;
// helper VSM
use App\Helpers\PreprocessingHelper as Preprocessing;
use App\Helpers\VSMHelper as VSM;

class KriminalController extends Controller
{
	// ====== PENGGUNAAN ====== //
    public function pencarian(Request $request)
    {
        // STEP 1 == query ke kata dasar (array)
        $query_dasar = Preprocessing::preprocess($request->cari);

        // STEP 2 = =get dokumen & parse to array
        $dokumen = DB::table('artikel')->get();
        $arrayDokumen = [];
        foreach ($dokumen as $d) {
            $arrayDoc = [
                "id_doc" => $d->id_blog,
                "dokumen" => implode(" ", Preprocessing::preprocess($d->deskripsi)),
            ];
            array_push($arrayDokumen, $arrayDoc);
        }

        // STEP 3 == get rank
        $rank = VSM::get_rank($query_dasar, $arrayDokumen);

        dd($rank);
    }
}
```

### Installing

A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
