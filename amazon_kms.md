# Amazon KMS

* Encrypting data,
    * **In-flight** (HTTPS/SSL/TLS)
    * **At rest** (Server-Side Encryption)
        * Data encryption is done using a DATA Key when received and before been stored.
        * Data decryption is done when the data is requested and before been sent.
    * **Client-Side**:
        * Data is encrypted at client side before being sent and decrpyted when received.
        * It leverages Envelope Encryption.
        * Server can't decrypt the data.
    * SSE-C (Server-side encryption using customer provided encryption keys)

* CMK (Customer Master Keys)
    * **Symmetric Keys** (AES-256 Keys)
        * Single key is used for encryption/decryption
        * Necessary for Envelope Encryption.
    * **Asymmetric Keys** (RSA, ECC) 
        * Public(Encryption) / Private(Decryption) keys.
    * **Types (Management Wise)**:
        * **Customer managed CMKs**
            * `DescribeKey` API response has `KeyManager` value as `CUSTOMER`
            * Resides in your AWS Account. 
        * **AWS managed CMKs**
            * `DescribeKey` API response has `KeyManager` value as `AWS`
            * Resides in your AWS Account.
        * **AWS owned CMKs**
            * Used by AWS Services to protect the resources in your account.
            * Does not reside in your AWS Account.
            * Cannot be viewed/tracked/audited.
    * Usage audit can be done using `CloudTrail` logs.
    * Maximum data size supported by KMS is `4` KB. Anything over `4` KB, `AWS Encryption SDK` should be used.
* **Data Keys** (For data **> 4KB** in size)
    * CMKs can be used to generate (`GenerateDataKey`), encrypt and decrypt data keys.
    * Managed outside of AWS KMS.
    * Create a Data Key using `GenerateDataKey` with a specified CMK (Customer Master Key):
        * **GenerateDataKey** returns,
            * A `plaintext` copy of the `DataKey`.
            * Another copy of the `DataKey` encrypted under `CMK`.
        * **GenerateDataKeyWithoutPlaintext** can also be used to generate only encrypted DataKey.
    * To use `DataKey` we need to ask AWS KMS to **decrypt** it.
    * **Encrypt** data with `DataKey`:
        *  Plaintext data key `-->` Unencrypted data `-->` Encrypted data 
        *  Encrypted data key `-->` AWS KMS Decrypt data key `-->` Plaintext data key `-->` Unencrypted data `-->` Encrypted data
        *  **Note:** Encrypted data key can also be stored along with the encrypted data. It is known as `Envelope Encryption`  
    * **Decrypt** data with `DataKey`:
        * Encrypted data key --> AWS KMS Decrypt data key `-->` Plaintext data key `-->` Encrypted data `-->` Unencrypted data 
    
* Resource Quotas: [here](https://docs.aws.amazon.com/kms/latest/developerguide/resource-limits.html)
* Request Quotas: [here](https://docs.aws.amazon.com/kms/latest/developerguide/requests-per-second.html)
    
        
        
        
        
          
     
    