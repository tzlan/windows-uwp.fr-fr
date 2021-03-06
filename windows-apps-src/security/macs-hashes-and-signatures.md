---
title: Codes d’authentification des messages, hachages et signatures
description: Cet article explique comment les codes d’authentification de message (MAC), les codes de hachage et les signatures peuvent être utilisés dans les applications UWP pour détecter une falsification des messages.
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 7932636679a09834b982e2c320309b90c759c365
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167143"
---
# <a name="macs-hashes-and-signatures"></a>Codes d’authentification des messages, hachages et signatures




Cet article explique comment les codes d’authentification de message (MAC), les codes de hachage et les signatures peuvent être utilisés dans les applications UWP pour détecter une falsification des messages.

## <a name="message-authentication-codes-macs"></a>Codes d’authentification des messages


Le chiffrement permet d’empêcher un individu non autorisé de lire un message, mais il n’empêche pas cet individu de le falsifier. Un message altéré, même si l’altération ne produit rien d’autre qu’une absurdité, peut avoir des coûts réels. Un code d’authentification des messages (MAC) permet d’empêcher la falsification des messages. Par exemple, considérez le scénario suivant :

-   Bob et Alice partagent une clé secrète et conviennent d’une fonction MAC à utiliser.
-   Bob crée un message, puis entre le message et la clé secrète dans une fonction MAC pour récupérer une valeur MAC.
-   Bob envoie le \[ message non chiffré \] et la valeur Mac à Alice sur un réseau.
-   Alice utilise la clé secrète et le message comme entrée pour la fonction MAC. Elle compare la valeur MAC générée à la valeur MAC envoyée par Bob. Si elles sont identiques, cela indique que le message n’a pas été modifié lors du transit.

Notez qu’Eve, une tierce personne effectuant une écoute clandestine de la conversation entre Bob et Alice, ne peut effectivement pas manipuler le message. Eve n’a pas accès à la clé privée et, par conséquent, ne peut pas créer de valeur MAC qui ferait apparaître le message falsifié comme légitime à Alice.

La création d’un code d’authentification des messages garantit que le message d’origine n’a pas été altéré et, en utilisant une clé secrète partagée, que le hachage du message a été signé par une personne ayant accès à cette clé privée.

Vous pouvez utiliser [**MacAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider) pour énumérer les algorithmes Mac disponibles et générer une clé symétrique. Vous pouvez utiliser des méthodes statiques sur la classe [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) pour effectuer le chiffrement nécessaire qui crée la valeur Mac.

Les signatures numériques sont l’équivalent, pour les clés publiques, des codes d’authentification de message (MAC) des clés privées. Bien que les codes d’authentification des messages (MAC, Message Authentication Codes) utilisent des clés privées pour permettre au destinataire d’un message de vérifier qu’un message n’a pas été altéré pendant la transmission, les signatures utilisent une paire clé privée/clé publique.

Cet exemple de code montre comment utiliser la classe [**MacAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider) pour créer un code d’authentification de message haché (HMAC).

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>Codes de hachage


Une fonction de hachage de chiffrement prend un long bloc de données au hasard et renvoie une chaîne de bits de taille fixe. Les fonctions de hachage sont généralement utilisées dans le cadre de la signature de données. Comme la plupart des opérations de signature de clé publique sont gourmandes en calcul, il est plus efficace de signer (chiffrer) un hachage de message plutôt que le message d’origine. La procédure suivante représente un cas courant, bien que simplifié :

-   Bob et Alice partagent une clé secrète et conviennent d’une fonction MAC à utiliser.
-   Bob crée un message, puis entre le message et la clé secrète dans une fonction MAC pour récupérer une valeur MAC.
-   Bob envoie le \[ message non chiffré \] et la valeur Mac à Alice sur un réseau.
-   Alice utilise la clé secrète et le message comme entrée pour la fonction MAC. Elle compare la valeur MAC générée à la valeur MAC envoyée par Bob. Si elles sont identiques, cela indique que le message n’a pas été modifié lors du transit.

Notez qu’Alice a envoyé un message non chiffré. Seul le hachage était chiffré. La procédure permet uniquement de s’assurer que le message d’origine n’a pas été modifié et, à l’aide de la clé publique d’Alice, que le hachage du message a été signé par quelqu’un qui a accès à la clé privée d’Alice, probablement Alice elle-même.

Vous pouvez utiliser la classe [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) pour énumérer les algorithmes de hachage disponibles et créer une valeur [**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash).

Les signatures numériques sont l’équivalent, pour les clés publiques, des codes d’authentification de message (MAC) des clés privées. Alors que les codes MAC utilisent des clés privées pour permettre au destinataire d’un message de vérifier que celui-ci n’a pas été modifié au cours de transmission, les signatures utilisent une paire de clés privée/publique.

L’objet [**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash) peut être utilisé pour hacher plusieurs fois des données différentes sans avoir à recréer l’objet pour chaque utilisation. La méthode [**Append**](/uwp/api/windows.security.cryptography.core.cryptographichash.append) ajoute de nouvelles données à une mémoire tampon en vue d’être hachées. La méthode [**GetValueAndReset**](/uwp/api/windows.security.cryptography.core.cryptographichash.getvalueandreset) hache les données et réinitialise l’objet pour une autre utilisation. En voici une illustration dans l’exemple suivant.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>Signatures numériques


Les signatures numériques sont l’équivalent, pour les clés publiques, des codes d’authentification de message (MAC) des clés privées. Alors que les codes MAC utilisent des clés privées pour permettre au destinataire d’un message de vérifier que celui-ci n’a pas été modifié au cours de transmission, les signatures utilisent une paire de clés privée/publique.

Comme la plupart des opérations de signature de clé publique sont gourmandes en calcul, il est toutefois plus efficace de signer (chiffrer) un hachage de message plutôt que le message d’origine. L’expéditeur crée un hachage de message, le signe et envoie la signature et le message (non chiffré). Le destinataire calcule un hachage sur le message, déchiffre la signature et compare la signature déchiffrée à la valeur de hachage. Si elles correspondent, le destinataire peut être quasi certain que le message provient effectivement de l’expéditeur et qu’il n’a pas été modifié au cours de la transmission.

La signature permet uniquement de s’assurer que le message d’origine n’a pas été modifié et, à l’aide de la clé publique de l’expéditeur, que le hachage du message a été signé par quelqu’un qui a accès à la clé privée.

Vous pouvez utiliser un objet [**AsymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider) pour énumérer les algorithmes de signature disponibles et générer ou importer une paire de clés. Vous pouvez utiliser des méthodes statiques sur la classe [**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash) pour signer un message ou vérifier une signature.