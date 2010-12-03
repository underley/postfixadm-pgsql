UID       = 0
GID       = 113         

DBSETTING = 'dbconfig.cf'
QUERY_MAP = {
  'relay_domains'               => 'SELECT domain FROM domain WHERE domain=\'%s\' and backupmx = true',
  'virtual_alias_maps'          => 'SELECT goto FROM alias WHERE address=\'%s\' AND active = true',
  'virtual_domains_maps'        => 'SELECT domain FROM domain WHERE domain=\'%s\' and backupmx = false and active = true',
  'virtual_mailbox_limits'      => 'SELECT quota FROM mailbox WHERE username=\'%s\'',
  'virtual_mailbox_maps'        => 'SELECT domain || \'/\' || maildir FROM mailbox WHERE username=\'%s\' AND active = true'
}

desc "generate config files"
task :default do
  dbcfg = File.read DBSETTING

  QUERY_MAP.each do |name, query|
    File.open "#{name}.cf", File::CREAT|File::TRUNC|File::RDWR, 0640 do |f|
      f.write dbcfg
      f.write "\n"
      f.write "query = #{query}"
      f.write "\n"
      f.close
    end

    File.chown UID, GID, "#{name}.cf"
  end
end
