# blockchain_darbas_3

## Libbitcoin-system bibliotekos diegimas

Pagal šią [nuorodą](https://github.com/libbitcoin/libbitcoin-system?tab=readme-ov-file#windows) galima įsidiegti Libbitcoin-system biblioteką ir ją lapaikančius įrankius. Klonavus repozitoriją, pagal ```source\repos\libbitcoin-system\builds\msvc\build``` į build aplankalą reikia įkelti jūsų parsisiųstą ```nuget.exe``` ([nuoroda](https://www.nuget.org/downloads)). Tada paspaudus ```build_all.bat``` parsiunčiami visi reikalingi NuGet paketai. 

## Darbas su biblioteka
Susikūriau naują failą ir įklijavau kodą:<br>
```
  #include <bitcoin/bitcoin.hpp>
  bc::hash_digest create_merkle(bc::hash_list& merkle)
{
 if (merkle.empty())		
  return bc::null_hash;	
 else if (merkle.size() == 1)	
  return merkle[0];	
 while (merkle.size() > 1)
	{		
  if (merkle.size() % 2 != 0)
   merkle.push_back(merkle.back());
   assert(merkle.size() % 2 == 0);
   bc::hash_list new_merkle;
   for (auto it = merkle.begin(); it != merkle.end(); it += 2)
		{
   bc::data_chunk concat_data(bc::hash_size * 2);
   auto concat = bc::serializer<
  decltype(concat_data.begin())>(concat_data.begin());
			concat.write_hash(*it);
			concat.write_hash(*(it + 1));
bc::hash_digest new_root = bc::bitcoin_hash(concat_data);
   new_merkle.push_back(new_root);
		}
  merkle = new_merkle;
		// DEBUG output -------------------------------------
		std::cout << "Current merkle hash list:" << std::endl;
		for (const auto& hash : merkle)
			std::cout << " " << bc::encode_base16(hash) << std::endl;
		std::cout << std::endl;
		// --------------------------------------------------
	}
	// Finally we end up with a single item.
	return merkle[0];
}
int main()
{
	// Transactions hashes from a block (#100 000) to reproduce the same merkle root
	bc::hash_list tx_hashes{ {
	bc::hash_literal("8c14f0db3df150123e6f3dbbf30f8b955a8249b62ac1d1ff16284aefa3d06d87"),
	bc::hash_literal("fff2525b8931402dd09222c50775608f75787bd2b87e56995a7bdd30f79702c4"),
	bc::hash_literal("6359f0868171b1d194cbee1af2f16ea598ae8fad666d9b012c8ed2b79a236ec4"),
	bc::hash_literal("e9a66845e05d5abc0ad04ec80f774a7e585c6e8db975962d069a522137b80c1d"),
	} };
	const bc::hash_digest merkle_root = create_merkle(tx_hashes);
	std::cout << "Merkle Root Hash: " << bc::encode_base16(merkle_root) << std::endl;
	// std::cout << "Merkle Root Hash-2: " << bc::encode_hash(merkle_root) << std::endl;
	return 0;
}
```
Bibliotekos neranda, todėl einame į ```Project / Properties / C/C++ / General / Additional Include Directories``` čia parenkame kelią į ```source\repos\libbitcoin-system\include\bitcoin```. <br>
Pačiame kode tenka pakeisti ```#include <bitcoin/bitcoin.hpp>``` į ```#include <bitcoin/system.hpp>```, kadangi ```bitcoin.hpp``` nėra. Dėja, bibliotekos nerado.
'
<br>
Tada Paprašiau pagalbos iš kursiokių. Buvo nuspręsta naudoti linux. Pagal šią [nuorodą](https://learn.microsoft.com/en-us/windows/wsl/install) galima tai padaryti. Į cmd įrašiau ```wsl --install``` ir perkroviau kompiuterį. Perkrovus, iš karto atsidaro ubuntu. Tada yra paprašoma susikurti prisijungimą.
![image](https://github.com/user-attachments/assets/baa99640-338e-4782-a1ba-dbb9b0cca47f)
Tada naudojausi kursiokės [readme failu](https://github.com/paulynaa/Blockchain-Papildoma/tree/3papildoma?tab=readme-ov-file) ir suvedžiau reikalingas komandas į terminalą. Tačiau gaunu tokią klaidą:
![image](https://github.com/user-attachments/assets/04d95ffa-620c-4f36-9844-ab048ae6ef49)
Po daug įvairių bandymų pavyko.
Sukūriau failą su ```nano merkle.cpp```
![image](https://github.com/user-attachments/assets/6bea7c74-fd88-4907-8923-23b564c25893)
Tačiau, kai norėjau paleisti kodą gavau error
![image](https://github.com/user-attachments/assets/21fa3d1f-5add-4b5e-af4e-32a15e7e9bc1)










