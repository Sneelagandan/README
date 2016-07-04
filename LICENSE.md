
// Set up the JAAS provider (IDTrust) and a policy file (which defines the "dna-jcr" login config name)

IDTrustConfiguration idtrustConfig = new IDTrustConfiguration();

try {

    idtrustConfig.config("security/jaas.conf.xml");

} catch (Exception ex) {

    throw new IllegalStateException(ex);

}


// Now configure the repository client component ...

RepositoryClient client = new RepositoryClient();

for (String arg : args) {

    arg = arg.trim();

    if (arg.equals("--api=jcr")) client.setApi(Api.JCR);

    if (arg.equals("--api=dna")) client.setApi(Api.DNA);

    if (arg.equals("--jaas")) client.setJaasContextName(JAAS_LOGIN_CONTEXT_NAME);

    if (arg.startsWith("--jaas=") && arg.length() > 7) client.setJaasContextName(arg.substring(7).trim());

}

// And have it use a ConsoleInput user interface ...

client.setUserInterface(new ConsoleInput(client, args));
