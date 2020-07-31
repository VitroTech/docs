# Report From Work Done On Signature Verification Issue

> Problem is described within [Vitro Oracle PoC Release Notes](https://github.com/VitroTech/release-notes/blob/2fb0391512e492c5d875828b1b4b90d98557e8b4/2020/oracle-poc-2020-07-22/release-notes.md).

### Steps Done

 1. Research on core concepts related to the issue. As it's a more complex
 Issue it needed time,
 2. After getting AWS credentials needed to run the ___Oracle Data Store___,
    the _Oracle Data Store_ and _Oracle Key Store PoC_ was build,
 3. With the help of components creator, the components were configured to work
    with each other and some potential fixes were tested (listed below) - none
    of which did bring the expected result,

### What Was Checked

  * In `vitro-oracle-key-store-poc/app/decryption_module.js` in function named
 `verifySignature`:
    ```
    let decryptPayload = async (payload, key) => {
    console.log("decryption-module: decrypt payload :",payload.length, payload)
    console.log("decryption-module: decrypt payload using key:",key.length,
    key, "(binary:",key.toString('binary'),",)")

    const iv = payload.slice(0,BLOCK_SIZE)
    const content = payload.slice(BLOCK_SIZE)

    console.log("decryption-module: iv :", iv)
    console.log("decryption-module: content :", content)
    const decipher = crypto.createDecipheriv(ENCRYPTION_ALGORITHM, key, iv);   
    decipher.setAutoPadding(false);
    let decrypted = decipher.update(content,'binary','binary');
    decrypted += decipher.final('binary');
    return decrypted
    ```
    * in `verify = crypto.createVerify('SHA256')` - different hash algorithms
      (e.g. `RSA-SHA256`),
    * different initialization of the `Buffer` module,
    * rewriting of `verifySignature` and `decryptPayload` functions to add
console logs meant to help with tracking the issue,
  * In `vitro-oracle-data-store/cmd/iot-msg-sender/main.go` in function named
 `sendSingleIotBlock`:
    * multiple RS keys joining combinations in `payloadSigned` (**the way it is
       done now is probably the main problem**). Code snippet:
    ```
    fmt.Printf("signature: (0x%x, 0x%x)\n", r, s)
  	// Concatenate signature with payload
  	payloadSigned := make([]byte, 0)
  	payloadSigned = append(payloadSigned, r.Bytes()...)
  	payloadSigned = append(payloadSigned, s.Bytes()...)
  	fmt.Println("signature takes first:", len(payloadSigned), " bytes of
    payload")
  	payloadSigned = append(payloadSigned, []byte(payload)...)
  	fmt.Println("total payload size:", len(payloadSigned))
    ```


### Conclusion

As there was no additional possible solutions in sight there was no further
attempts made. Documentation is scarce on this matter, so it's hard to find an
description on how to attach the keys to the payload.
